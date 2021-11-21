---
title:  "Pattern - Factory Method"
excerpt: "Gof Design Pattern"

categories:
  - Pattern
tags:
  - Pattern
sidebar:
  nav: "pattern"
classes: wide
last_modified_at: 2019-04-13T08:06:00-05:00
---

> 빨리 움직여 문제점을 해결하라. 당신이 문제점을 해결하지 않으면 당신을 빨리 나아갈 수 없을 것이다.
>   - 마크 주거버그 ( 페이스북 CEO )

# 의도 

***

객체를 생성하기 위해서 인터페이스를 정의하지만, 어떤 클래스의 인스턴스를 생성할 지에 대한 결정은 서브 클래스가 내리도록 합니다. Factory 하나에 대한 객체의 종류에 따라 인스턴스를 반환할 수 있게 처리합니다. 

# 활용성

***

프레임 워크는 추상 클래스를 사용하여 객체 간의 관련성을 정의하고 유지할 수 있습니다. 또한 프레임워크는 이들 객체를 생성할 책임을 지니기도 합니다.
프레임 워크는 이들 추상 클래스 간의 상호작용을 책임져서 전체 시스템의 기본 동작 방식을 정의힙니다.

```java

// 아래의 코드는 Framework에서 제공하는 추상부에 대해서 실제 구현을 개발자에게 맞기고 이를 Framework에서 활용하는 방식이다. 

@Configuration
@EnableWebMvc
public class WebMvcConfig implements WebMvcConfigurer {
    @Override
    public void addFormatters(FormatterRegistry formatterRegistry) {
        formatterRegistry.addConverter(new MyConverter());
    }

    @Override
    public void configureMessageConverters(List<HttpMessageConverter> converters) {
        converters.add(new MyHttpMessageConverter());
    }
}

```

아래의 Package 설계 범위를 보면, 핵심 뼈대와 실제 구현을 분리한다. Framework 개발 시에 기본 틀/구성에 대해서 고민해볼 수 있는 구조이다.

![](https://keepinmindsh.github.io/lines/assets/img/factory_method_folder_structure.png){: .align-center}

- 기본 설계 구조

![](https://keepinmindsh.github.io/lines/assets/img/factory_method.png){: .align-center}

- 상세 설명
  - Product ( Document ) : 팩토리 메서드가 생성하는 객체의 인터페이스를 정의합니다.
  - ConcreteProduct( MyDocument ) : Product 클래스에 정의된 인터페이스를 실제로 구현합니다.
  - Creator ( Application ) : Product 타입의 객체를 반환하는 팩토리 메서드를 선언합니다.
  - Creator 클래스는 팩토리 메서드를 기본적으로 구현하는데, 이 구현에서는 ConcreteProduct객체를 반환합니다. 또한 Product 객체의 생성을 위해 팩토리 메서드를 호출합니다.
  - ConcreteCreator ( MyApplication ) : 팩토리 메서드를 재정의하여 ConcreteProduct의 인스턴스를 반환합니다.

- 팩토리 메서드를 사용하는 이점
  - 어떤 클래스가 자신이 생성해야 하는 객체의 클래스를 예측할 수 없을 때,
  - 생성할 객체를 기술하는 책임을 자신의 서브 클래스가 지정했으면 할 때,
  - 객체 생성의 책임을 몇 개의 보조 서브 클래스 가운데 하나에게 위임하고, 어떤 서브 클래스가 위임자인지에 대한 정보를 국소화 시키고 싶을 때

# 코드 예제 - 기본

***

```java

package AbstractFactory;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.HashMap;
import java.util.Map;

public abstract class AbstractSendor {

    public void sendDataToInterface(Object param, QuerySelector qeurySelector) throws SQLException{
    Map connectionMap = (HashMap)param;

    Connection conn = null;
    PreparedStatement psmt = null;
    ResultSet rs = null;

    conn = getConnection(connectionMap);

    psmt = getStatment(conn, qeurySelector.excuteQueryByString(connectionMap.get("queryID")));

    rs = executeQuery(connectionMap);

    printResult(rs);
    }

    public abstract Connection getConnection(Object param);

    public abstract PreparedStatement getStatment(Connection conn, String stringQuery);

    public abstract ResultSet executeQuery(Map parameter);

    public abstract void printResult(ResultSet rs) throws SQLException;
}                       
    

```

```java

package FactoryMethod;

import java.sql.Connection;

public class FactoryMethod {
    public static void main(String[] args) {
        DBCaller dbCaller = new OracleDBCaller();

        dbCaller.AnOperation();
    }
}


interface Connector{
    public Connection DBConnector();
}


class OracleConnector implements Connector{

    public Connection DBConnector() {
        return null;
    }
}   

class MssqlConenctor implements Connector{
    public Connection DBConnector(){
        return null;
    }
}

abstract class DBCaller {

abstract Connector CallDBConnector();

    public void AnOperation(){
        CallDBConnector();
    }
}

class OracleDBCaller extends DBCaller{

    @Override
    Connector CallDBConnector() {
        return new OracleConnector();
    }
}  

```

# 코드 예제 - 활용 

***

- 마린의 훈련 과정
  - 마린이 기본적으로 배워할 기능에 대해서 Barrack을 통해서 배울 수 있어야 한다.
  - 이는 기계가 아닌 지상 유닛의 경우 공격형 유닛에 대해서 모두 해당된다.
- 요구 사항의 공통화
  - 지상 유닛이 동일하게 처리하는 프로세스에 대해서 공통화/추상화
{: .notice--info}

```java
package DesignPattern.gof_factoryMethod.sample001;

    public abstract class SoldierUnitCreator {

    public Soldier createUnit(Soldier unit) {

        enhanceHealthSoldier(unit);

        EquipmentWear(unit);

        EquipmentWeapon(unit);

        BattleTraining(unit);

        return unit;
    }

    protected abstract void enhanceHealthSoldier(Soldier soldier);

    protected abstract void EquipmentWear(Soldier soldier);

    protected abstract void EquipmentWeapon(Soldier soldier);

    protected abstract void BattleTraining(Soldier soldier);
}      
```

```java
package DesignPattern.gof_factoryMethod.sample001;

public class Barrack extends SoldierUnitCreator {

    @Override
    protected void enhanceHealthSoldier(Soldier soldier) {

    }

    @Override
    protected void EquipmentWear(Soldier soldier) {

    }

    @Override
    protected void EquipmentWeapon(Soldier soldier) {

    }

    @Override
    protected void BattleTraining(Soldier soldier) {

    }
} 
```
```java
package DesignPattern.gof_factoryMethod.sample001;

public interface Unit {

    public void attack();

    public void destroyed();

    public void depend();
}
```

```java
package DesignPattern.gof_factoryMethod.sample001;

public interface Soldier extends Unit {
    void getEnhancePower();

    void getEquipmentWear();

    void getEquipmentWeapon();

    void getBattleTraining();
}  
```

```java
package DesignPattern.gof_factoryMethod.sample001;

public class Marine implements Soldier {
    public void getEnhancePower() {

    }

    public void getEquipmentWear() {

    }

    public void getEquipmentWeapon() {

    }

    public void getBattleTraining() {

    }

    public void attack() {

    }

    public void destroyed() {

    }

    public void depend() {

    }
}                           
```
```java

package DesignPattern.gof_factoryMethod.sample001.application;

import DesignPattern.gof_factoryMethod.sample001.*;

public class TraningCenter {

    public static Soldier createUnitOrNull(String UnitType){

        switch (UnitType){
            case "Marine" :
                SoldierUnitCreator barrack = new Barrack();

                Soldier marine1 = new Marine();

                return barrack.createUnit(marine1);
            default:
                return null;
        }
    }
} 

```

```java
package DesignPattern.gof_factoryMethod.sample001.application;

import DesignPattern.gof_factoryMethod.sample001.Soldier;

    public class Client {

    public static void main(String[] args) {
        Soldier marine1 = TraningCenter.createUnitOrNull("Marine");

        marine1.attack();

        marine1.getBattleTraining();
        marine1.getEnhancePower();
    }
}  
```

# 코드 예제 - 팩토리 메소드 효용 

***

팩토리 메소드 패턴을 사용하는 이유는 클래스간의 결합도를 낮추기 위한것입니다.
결합도라는 것은 간단히 말해 클래스의 변경점이 생겼을 때 얼마나 다른 클래스에도 영향을 주는가입니다.
팩토리 메소드 패턴을 사용하는 경우 직접 객체를 생성해 사용하는 것을 방지하고 서브 클래스에 위임함으로써 보다 효율적인 코드 제어를 할 수 있고 의존성을 제거합니다.
결과적으로 결합도 또한 낮출 수 있습니다.
{: .notice--info}

```java

package DesignPattern.gof_factoryMethod.client.Sample002Application;

import DesignPattern.gof_factoryMethod.client.Sample002Application.ACCToWav.AACFile;
import DesignPattern.gof_factoryMethod.client.Sample002Application.ACCToWav.WavAudioConverter;
import DesignPattern.gof_factoryMethod.sample002.AudioConverter;
import DesignPattern.gof_factoryMethod.sample002.MFCCObject;
import DesignPattern.gof_factoryMethod.client.Sample002Application.ACCToWav.WavConfig;

public class Client {

    public static void main(String[] args) {

        WavConfig.Builder wavConfig = new WavConfig.Builder("/home/user/");

        AudioConverter wavConverter = new WavAudioConverter(wavConfig.setWavFileName("wav.wav").build());

        MFCCObject mfccObject = wavConverter.parsingAudio(new AACFile());
    }
}  

```

```java

package DesignPattern.gof_factoryMethod.client.Sample002Application.ACCToWav;

import DesignPattern.gof_factoryMethod.sample002.Config;

public class WavConfig implements Config {
  private final String wavFilePath;

  private final String wavFileName;

  public static class Builder {

      // 필수 값
      private final String wavFilePath;

      private String wavFileName;

      public Builder(String wavFilePath){
          this.wavFilePath = wavFilePath;
      }

      public Builder setWavFileName(String wavFileName){
          wavFileName = wavFileName;
          return this;
      }

      public WavConfig build(){
          return new WavConfig(this);
      }
  }

  private WavConfig(Builder builder){
      wavFilePath = builder.wavFilePath;
      wavFileName = builder.wavFileName;
  }

  public String getWavFileName() {
      return wavFileName;
  }
} 

```

```java

package DesignPattern.gof_factoryMethod.client.Sample002Application.ACCToWav;

import DesignPattern.gof_factoryMethod.sample002.AudioFile;

public class AACFile implements AudioFile {
}

```

```java

package DesignPattern.gof_factoryMethod.client.Sample002Application.ACCToWav;

import DesignPattern.gof_factoryMethod.sample002.AudioConverter;
import DesignPattern.gof_factoryMethod.sample002.AudioFile;
import DesignPattern.gof_factoryMethod.sample002.Config;
import DesignPattern.gof_factoryMethod.sample002.MFCCObject;

public class WavAudioConverter extends AudioConverter {

    private Config config = null;

    public WavAudioConverter(Config config){
        this.config = config;
    }

    protected Config AudioFileByConverter(AudioFile audioFile) {

        config.getWavFileName();

        return config;
    }

    protected MFCCObject AnalizeAudioFile(Config config) {
        return null;
    }
}    

```

```java

package DesignPattern.gof_factoryMethod.client.Sample002Application.ACCToWav;

import DesignPattern.gof_factoryMethod.sample002.MFCCObject;

public class WavMFCCObject implements MFCCObject {

}

```

```java
package DesignPattern.gof_factoryMethod.sample002;

public abstract class AudioConverter {

protected abstract Config AudioFileByConverter(AudioFile audioFile);

protected abstract MFCCObject AnalizeAudioFile(Config audioFile);

    public MFCCObject parsingAudio(AudioFile audioFile){

        Config config = AudioFileByConverter(audioFile);

        return AnalizeAudioFile(config);
    }
}  

```

```java
package DesignPattern.gof_factoryMethod.sample002;

public interface AudioFile {
}   

```

```java

package DesignPattern.gof_factoryMethod.sample002;

public interface Config {

    public String getWavFileName();

}

```

```java

package DesignPattern.gof_factoryMethod.sample002;

public interface MFCCObject {

}   

```

# 코드 예제 - Static Factory Method 의 활용 

***

- 생성자 대신 정적 팩터리 메서드를 사용할 수 없는지 생각해 보라.

  - 정적 팩터리 메서드를 사용하면, 불변 객체에 대해서 new 생성자가 아닌 캐시해서 사용이 가능하다.

  ```java

  public static final BigInteger ZERO = new BigInteger(new int[0], 0);

  private final static int MAX_CONSTANT = 16;
  private static BigInteger posConst[] = new BigInteger[MAX_CONSTANT+1];
  private static BigInteger negConst[] = new BigInteger[MAX_CONSTANT+1];
  
  public static BigInteger valueOf(long val) {
      if (val == 0)
          return ZERO;
      if (val > 0 && val <= MAX_CONSTANT)
          return posConst[(int) val];
      else if (val < 0 && val >= -MAX_CONSTANT)
          return negConst[(int) -val];
  
      return new BigInteger(val);
  }

  ```

  - 정적 팩터리 메서드를 사용하면, 가독성이 높아진다

  ```java

    public class WarpCharacter {
        public static Airship InterceptorWarp(Map mapObj , int xPostion, int yPostion){
            return new Interceptor();
        }

        public static Airship ScouterWarp(Map mapObj , int xPostion, int yPostion) {
            return new Scouter();
        }

        public static Airship ArbiterWarp(Map mapObj , int xPostion, int yPostion){
            return new Arbiter();
        }
    }     

  ```

  - 정적 팩터리 메서드를 사용하면, 다형성의 원칙에 따라 하위 자료형을 사용하게 만들 수 있다

  ```java

    package DesignPattern.gof_factoryMethod.staticfactorymethod;

    public interface Airship {

        public void Attack();

        public void Retreat();

        public void Destory();
    }

    package DesignPattern.gof_factoryMethod.staticfactorymethod;

    public class Arbiter implements Airship {
        public void Attack() {

        }

        public void Retreat() {

        }

        public void Destory() {

        }
    }

    package DesignPattern.gof_factoryMethod.staticfactorymethod;

    public class WarpCharacter {
        public static Airship InterceptorWarp(Map mapObj , int xPostion, int yPostion){
            return new Interceptor();
        }

        public static Airship ScouterWarp(Map mapObj , int xPostion, int yPostion) {
            return new Scouter();
        }

        public static Airship ArbiterWarp(Map mapObj , int xPostion, int yPostion){
            return new Arbiter();
        }
    } 

  ```

  - 정적 팩터리 메서드를 사용하면, 형인자 자료형 객체를 만들 때 편리

  ```java


    // 정적 팩토리 메서드: type inference를 이용한다
    public static  HashMap newInstance() {
    return new HashMap();
    }              

    // 1.7 이후 부터는 아래와 같이 쓸수 있게 되면서 형인자 자료형 객체에 의한 정적 팩토리는 힘을 잃었다. 
    Map<String, List<String>> list = new HashMap<>();
                        

  ```

  - Client 에서 실제로 함수를 호출할 때

  ```java

    package DesignPattern.gof_factoryMethod.staticfactorymethod;

    public class Battle {

        public static void main(String[] args) {

            Map map = new Map();

            Airship arbiter1 =  WarpCharacter.ArbiterWarp(map, 10, 60);

            Airship interceptor1 = WarpCharacter.InterceptorWarp(map, 60, 800);

            Airship scouter1 = WarpCharacter.ScouterWarp(map, 300, 1024);

            arbiter1.Attack();

            interceptor1.Attack();

            scouter1.Attack();
        }
    }

  ```
