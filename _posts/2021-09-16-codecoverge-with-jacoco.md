---
title: "Jacoco를 활용한 CodeCoverage 방법 "
excerpt: "Jacoco를 gradle 기반에서 활용해보기"

categories:
  - Tool, CodeCoverage
tags:
  - Test, CodeCoverage
classes: wide
last_modified_at: 2021-07-29T07:31:00-05:00
---

> 모든 것이 다 잘 될 것이라고 생각하는 것은 착각이다.  

***

# Jacoco 

**JaCoCo Java Code Coverage Library**  

내가 생각하기에 Jacoco와 같은 Code Coverage 를 적용하는 이유는 명확하다. 우리가 작성하는 Test에 대한 Case가 모두 검출 되었는가를 개발자의 검증에 의해서 모두 체크하기에는 한계가 있기 마련이다. 따라서 이를 자동화하여 테스트 케이스를 검출할 수 있는 역할을 할 수 있다. 
다만 Code Coverage가 모든 경우의 테스트 케이스를 검출하는 것을 불가능하지만 적어도 나의 Test Code가 모든 코드 라인을 통과하여 테스트가 수행이 가능한가에 대한 확신을 얻을 수는 있다.  

- JaCoCo is a free code coverage library for Java, which has been created by the EclEmma team based on the lessons learned from using and integration existing libraries for many years.  

- [번역] Jacoco는 자바를 위한 무료 코드 커버리지 라이브러리이다. 

코드 커버리지란 무엇인가를 이해하려면 기본 동작 원리 이해할 필요가 있다.  

개발자가 자바 소스코드를 작성하고, 자바 컴파일러가 자바 소스 파일을 컴파일 한다. 이 때 나오는 파일은 자바 바이트 코드로 치환된 class 파일이 생성된다. 여기에서 자바 바이트코드로 작성된 .class을 기반으로 작성된 test가 동작할 때, 자바 바이트 코드 상에서 실제로 실행된 라인외에 실행되지 않은
라인을 기준으로 계산하여 Code Coverage 비율을 계산한다. 

### 기본 구성 환경 

- IntellJ
- Gradle 
- Java
- JUnit
- jacoco 0.8.7 <https://www.eclemma.org/jacoco/>

### 기본 샘플 

개발자 작성 코드 

```java

package lines.jacoco.sample;

public class JacocoCall {

    public String call(String value){
        if("Hello".equals(value)){
            return value;
        }else{
            return "Nothing";
        }
    }
}

```

Test 코드 

```java

package lines.jacoco.sample;

import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertEquals;

public class JacocoCallTest {

    private JacocoCall jacocoCall = new JacocoCall();

    @Test
    public void setJacocoCallTest(){
        String value = jacocoCall.call("Hello");

        assertEquals(value, "Hello");
    }
}

```

- build.gradle의 설정 

```gradle

plugins {
    id 'java'
    id 'jacoco'
}

group 'org.example'
version '1.0-SNAPSHOT'

repositories {
    mavenCentral()
}

dependencies {
    testImplementation 'org.junit.jupiter:junit-jupiter-api:5.7.0'
    testRuntimeOnly 'org.junit.jupiter:junit-jupiter-engine:5.7.0'
}

jacoco {
    toolVersion = "0.8.7"
    reportsDirectory = layout.buildDirectory.dir('customJacocoReportDir')
}

jacocoTestReport {
    reports {
        xml.required = false
        csv.required = false
        html.outputLocation = layout.buildDirectory.dir('jacocoHtml')
    }
}

jacocoTestCoverageVerification {
    // 이 커버리지 기준은 이 글의 맨 아래에서 다시 설명하겠습니다.
    violationRules {
        rule {
            element = 'CLASS'

            limit {
                counter = 'BRANCH'
                value = 'COVEREDRATIO'
                minimum = 0.90
            }
        }
    }
}

test {
    useJUnitPlatform()

    finalizedBy jacocoTestReport // report is always generated after tests run

    finalizedBy jacocoTestCoverageVerification // jacoco Test Coverage Verification after jacocoTestReport run
}

jacocoTestReport {
    dependsOn test // tests are required to run before generating the report
}

jacocoTestCoverageVerification{
    dependsOn test // tests are required to run before checking code coverage
}

```

### Code Coverage 의 결과를 웹에서 확인하기 

아래의 jacoco 설정을 통해서 build 폴더에 생성되는 HTML 자료에 의해서 결과 확인이 가능하다. 

```gradle 

jacoco {
    toolVersion = "0.8.7"
    reportsDirectory = layout.buildDirectory.dir('customJacocoReportDir')
}

jacocoTestReport {
    reports {
        xml.required = false
        csv.required = false
        html.outputLocation = layout.buildDirectory.dir('jacocoHtml')
    }
}

```

실제 build 되면 아래의 build 폴더 상의 구성을 확인할 수 있다. 

![](https://keepinmindsh.github.io/lines/assets/img/codecoverage_with_jacoco_02.png){: .align-center} 

### 기동시 실제 동작 결과 

여기에서 중요한 부분은 아래의 부분이다. 

Execution failed for task ':jacocoTestCoverageVerification'.  

위의 부분은 build gradle 상에서 test task가 단계적으로 실행 될 때 Test 코드의 Code Coverage 비율을 계산한다. 

```shell

오후 8:44:00: Executing task 'build'...

> Task :compileJava UP-TO-DATE
> Task :processResources NO-SOURCE
> Task :classes UP-TO-DATE
> Task :jar UP-TO-DATE
> Task :assemble UP-TO-DATE
> Task :compileTestJava UP-TO-DATE
> Task :processTestResources NO-SOURCE
> Task :testClasses UP-TO-DATE
> Task :test UP-TO-DATE
> Task :jacocoTestReport UP-TO-DATE

> Task :jacocoTestCoverageVerification FAILED
6 actionable tasks: 1 executed, 5 up-to-date
[ant:jacocoReport] Rule violated for class lines.jacoco.sample.JacocoCall: branches covered ratio is 0.50, but expected minimum is 0.90

FAILURE: Build failed with an exception.

* What went wrong:
Execution failed for task ':jacocoTestCoverageVerification'.
> Rule violated for class lines.jacoco.sample.JacocoCall: branches covered ratio is 0.50, but expected minimum is 0.90

* Try:
Run with --stacktrace option to get the stack trace. Run with --info or --debug option to get more log output. Run with --scan to get full insights.

* Get more help at https://help.gradle.org

BUILD FAILED in 1s
오후 8:44:02: Task execution finished 'build'.


```

실제 코드 커버리지의 비율이 아래의 이미지와 같기 때문에 build는 테스트가 실패한다. 

![](https://keepinmindsh.github.io/lines/assets/img/codecoverage_with_jacoco.png){: .align-center} 


코드 커버리지 90% 이상이 되지 않으면 해당 빌드는 성공할 수 없기 때문에 테스트 코드를 케이스에 맞게 잘 작성하거나, 코드 커버리지 범위를 조율할 필요가 있다. 해당 부분은 각 기업의 프로덕트의 구성에 따라 변경될 수 있는 부분이라고 생각된다. 