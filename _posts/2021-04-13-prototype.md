---
title:  "Pattern - Prototype"
excerpt: "Gof Design Pattern"

sidebar:
  nav: "pattern"
categories:
  - Pattern
tags:
  - Pattern 
classes: wide
last_modified_at: 2019-04-13T08:06:00-05:00
---

> 성공은 형편없는 선생님이다. 그것은 똑똑한 사람들로 하여금 절대 패할 수 없다고 착각하게 만든다.     
> - 빌게이츠 ( 마이크로소프트 공동 창업자 )

# 의도 

***

객체 생성과 관련된 패턴들은 일부 영역이 겹치는 부분들이 있는데, 프로트 타입 패턴은 주로 다른 객체 생성 패턴들과 많이 같이 사용될 수 있다. 추상 팩토리 패턴이 프로토타입들의 집합을 갖고 있다가, 클론(clone)한 프로덕트 객체를 반환할 수 있다. 일반화 관계로 표현을 할 때, 파생 클래스의 개수가 과도히 많아지고 각 클래스의 메서드가 수행하는 알고리즘에서 차이가 없고 생성 시에 개체의 속성에 차이만 있다면 원형 패턴을 사용하는 것이 효과적이다.

# 활용성

***

![](https://keepinmindsh.github.io/lines/assets/img/prototype_pattern.png){: .align-center}

Object 생성이 높은 비용으로 수 많은 요청을 하는 경우, 또는 비슷한 Object 를 지속적으로 생성해내야 할 때 유용하게 사용할 수 있다.

- 관련 패턴
  - 이번 장의 끝에서 잠깐 언급했듯이 원형 패턴과 추상 팩토리 패턴은 어떤 면에서는 경쟁적인 관계입니다. 하지만 함께 사용될 수도 있습니다.
  - 추상 팩토리 패턴은 원형 집합을 저장하다가 필요할 때 복제하여 제품 객체를 반환하도록 사용할 수 도 있습니다.

# 코드 예제 - 기본

***

```java
package DesignPattern.gof_prototype;
  
  public class Battle {
  
      public static void main(String[] args) {
  
          Unit prove1 = Nexus.createProve();
  
          prove1.Harvest();
          prove1.Harvest();
          prove1.Harvest();
  
          Arbiter arbiter1 = new Arbiter();
  
          Unit prove2 = arbiter1.copyRealUnit(prove1);
  
          System.out.println("prove 1 == prove 2  의 결과 : " + (prove1 == prove2) );
  
          System.out.println(prove1.getMineralCapacity());
  
          System.out.println(prove2.getMineralCapacity());
  
      }
  }                 
```

- 컴파일 결과 

![](https://keepinmindsh.github.io/lines/assets/img/prototype_compile.png){: .align-center}

# 코드 예제 - Deep Copy / Shallow Copy 

***

```java
package DesignPattern.gof_prototype;
    
public interface Unit{
    public void Harvest();

    public void Attack();

    public void Building();

    public Unit CloneUnitOrNull();

    public int getMineralCapacity();
}     
```

```java
package DesignPattern.gof_prototype;
        
public class Probe implements Unit{

  private int mineralCapacity = 0;

  public void Harvest(){
      System.out.println("자원을 캡니다.");

      mineralCapacity += 1;
  }

  public void Attack(){
      System.out.println("공격을 합니다.");
  }

  public void Building(){
      System.out.println("건물을 짓습니다.");
  }

  public int getMineralCapacity(){
      return mineralCapacity;
  }

  private void setCapacity(int capacity){
      this.mineralCapacity = capacity;
  }

  public Unit CloneUnitOrNull(){
      try {
          Object cloneObject = clone();

          return (Unit)cloneObject;

      }catch (CloneNotSupportedException e){
          e.printStackTrace();

          return null;
      }
  }

  @Override
  public Object clone() throws CloneNotSupportedException{

      Probe probe = new Probe();

      probe.setCapacity(mineralCapacity);

      return probe;
  }
}

```

```java

package DesignPattern.gof_prototype;
        
public class Nexus {
    public static Unit createProve(){
        return new Probe();
    }
}    

```

```java

package DesignPattern.gof_prototype;

public class Arbiter {
    public Unit copyRealUnit(Unit unit){
        return unit.CloneUnitOrNull();
    }
}  

```

# 코드 예제 - Clonable 의 Clone을 이용할 경우 

***

```java

 package DesignPattern.gof_prototype.sample002;
        
import java.util.List;

public class ComplexProcess implements Cloneable {


    private final Process complexProcess;

    public ComplexProcess(Process complexProcess){
        this.complexProcess = complexProcess;
    }

    public List<Object> getProcessResult(){
        return complexProcess.getResultOfComplexProcess();
    }

    public ComplexProcess getCloneProcess() throws CloneNotSupportedException {
        return (ComplexProcess)super.clone();
    }
}     

```

```java


package DesignPattern.gof_prototype.sample002;

import java.util.ArrayList;
import java.util.List;

public class CopyAthousandRowsProcess implements Process {

    public List<Object> getResultOfComplexProcess() {
        return new ArrayList<>();
    }
}  

```

```java

package DesignPattern.gof_prototype.sample002;

import java.util.List;

public interface Process {
    public List<Object> getResultOfComplexProcess();
}   

```

```java

package DesignPattern.gof_prototype.sample002;
        
public class Processor {

    public static void main(String[] args)  throws CloneNotSupportedException {

        ComplexProcess complexProcess1 = new ComplexProcess(new CopyAthousandRowsProcess());

        complexProcess1.getProcessResult();

        ComplexProcess complexProcess2 = complexProcess1.getCloneProcess();

        complexProcess2.getProcessResult();

        System.out.println("complexProcess1 == complexProcess1 의 결과는 " + (complexProcess1 == complexProcess2));
    }
}   

```
