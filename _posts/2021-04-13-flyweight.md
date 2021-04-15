---
title:  "Pattern - Flyweight"
excerpt: "Gof Design Pattern"

categories:
  - Pattern
tags:
  - Pattern 
classes: wide
last_modified_at: 2019-04-13T08:06:00-05:00
---

> 성공적인 직원 채용의 방법은 세상을 변화시키길 원하는 사람들을 찾아내는 것이다.
>   - 마크 베니오프 ( 세일즈 포스 CEO )

# 의도 

***

공유를 통해 많은 수의 소립 객체들을 효과적으로 지원합니다.

# 동기 

***

- 객체 중심으로 설계함으로써 많은 효과를 거둘 수 있기는 하지만 하단을 실제로 구현하는 비용이 만만치 않은 응용 프로그램이 있습니다.

문서 편집기에서 문자단위 하나까지 객체로 구성할 때 얻을 수 있는 확장성과 서식화, 영향성의 최소화가 가능하지만 여기에서 문제가 되는 것은 비용입니다. 그 이유는 얼마 되지 않는 문서일지라도 그 객체는 수천개의 문제 객체를 포함 할 수 있기 때문입니다. 이와 같은 문제를 플라이급(FlyWeight) 패턴을 이용해서 객체를 공유하는 방법을 통해 해결하는 방법을 보여줍니다.
플라이급 객체는 공유가능한 객체로, 여러 비슷한 상황에서 사용될 수 있습니다. 그러나 각각의 상황에서는 독립적인 객체로 동작합니다. 이것은 공유될 수 없는 객체의 인스턴스와 구분이 안된다는 의미입니다. 그러므로 플라이급 객체가 적용될 사항을 미리 가정하면서 소프트웨어를 개발할 수 없습니다. 즉 같은 것을 놓고 이런 상황에서는 이런 특징으로 정의하고, 또 다른 상황에서는 다른 특징으로 정의할 수 없다는 것입니다. 플라이급 패턴에서 중요한 개념은 본질적 상태와 부가적 상태의 구분입니다.
본질적 상태는 플라이급 객체에 저장되어야하며, 이것이 적용되는 상황과 상관없는 본질적 특성 정보들이 객체를 구성합니다.

# 활용성  

***

![](https://keepinmindsh.github.io/lines/assets/img/flyweight.png){: .align-center}

- 플라이급 패턴은 언제 사용하는가에 따라서 그 효과가 달라집니다.
  - 응용프로그램이 대량의 객체를 사용해야 할 때,
  - 객체의 수가 너무 많아져 저장 비용이 너무 높아질 때,
  - 대부분의 객체 상태를 부가적인 것으로 만들 수 있을 때,
  - 부가적인 속성들을 제거한 후 객체들을 조사해보니 객체의 많은 묶음이 비교적 적은 수의 공유된 객체로 대체될 수 있을 때. 현재 서로 다른 객체로 간주한 이유는 이들 부가적인 속성 때문이었지 본질이 달라던 것은 아닐 때,
  - 응용 프로그램이 객체의 정체성에 의존하지 않을 때. 플라이급 객체들은 공유될 수 있음을 의미하는데, 식별자가 있다는 것은 서로 다른 객체로 구별해야한다는 의미이므로 플라이급 객체를 사용할 수 없습니다.

# 코드 예제 

***

- Flyweight
*Glyph*
Flyweight가 받아들일 수 있고, 부가적인 상태에서 동작해야하는 인터페이스를 선언합니다.

- ConcreteFlyweight
*Character*
Flyweight 인터페이스를 구현하고 내부적으로 갖고 있어야 하는 본질적인 상태에 대한 저장소를 의미합니다. ConcreteFlyweight 객체는 공유할 수 있는 것이어야 합니다. 그러므로 관리하는 어떤 상태라도 본질적인 것이어야 합니다.

- UnsharedConcreteFlyweight
*Row, Column*
모든 플라이급 서브클래스들이 공유될 필요는 없습니다. Flyweight 인터페이스를 공유를 가능하게 하지만, 그것을 강요해서는 안됩니다. UnsharedConcreteFlyweight 객체가 ConcreteFlyweight 객체를 자신의 자식으로 갖는 것은 흔한일입니다.

- FlyweightFactory
플라이급 객체를 생성하고 관리하며 , 플라이급 객체가 제대로 공유되도록 보장합니다. 사용자가 플라이급 객체를 요청하면 FlywegihtFactory는 이미 존재하는 인스턴스를 제공하거나 만약 존재하지 않는다면 새로 생성합니다.

- Client
플라이급 객체에 대한 참조자를 관리하며 플라이급 객체의 부가적 상태를 저장합니다.

# 협력방법

***

- 플라이급 객체가 기능을 수행하는 데 필요한 상태가 본질적인 것인지 부가적인 것인지를 구분해야 합니다. 본질적 상태는 ConcreteFlyweight에 저장해야 하고, 부가적인 상태는 사용자가 저장하거나, 연산되어야 하는 다른 상태로 관리해야 합니다. 사용자는 연산을 호출할 때 자신에게만 필요한 부가적 상태를 플라이급 객체에 매개변수로 전달합니다.
- 사용자는 ConcreteFlyweight의 인스턴스를 직접 만들 수 없습니다. 사용자는 ConcreteFlyweight 객체를 FlyweightFactory 객체에서 얻어야 합니다. 이렇게 해야 플라이급 객체가 공유될 수 있습니다.
- 플라이급 패턴은 예전에는 모두 본질적인 상태로 저장되어 있던 것을 부가적인 상태로 만들어, 부가적인 상태의 연산과 전송에 드는 런타임 비용을 새로 들여올 수 있습니다. 하지만 이런 비용은 플라이급 객체의 공유를 통해 저장소를 절약이라는 반대급부를 가질 수도 있습니다. 저장소 절약은 여러 면에서 기능적입니다.
  - 공유해야 하는 인스턴스의 전체 수를 줄일 수 있습니다.
  - 객체별 본질적 상태의 양을 줄일 수 있습니다.
  - 부가적인 상태는 연산되거나 저장될 수 있습니다.

# 코드 예제 - 기본

***

```java
package designpattern.gof_flyweight.sample01;

public class FlyweightMain {

    private static INoodle[] ramen = new Ramen[20];
    private static NoodleContext[] tables = new NoodleContext[20];
    private static int ordersCount = 0;
    private static NoodleFactory noodleFactory;

    public static void takeOrder(String flavorIn, int table) {
        ramen[ordersCount] = noodleFactory.getNoodleFlavor(flavorIn);
        tables[ordersCount] = new NoodleContext(table);
        ordersCount++;
    }

    public static void main(String args[]) {
        noodleFactory = new NoodleFactory();

        takeOrder("Zin Ramen", 2);
        takeOrder("Zin Ramen", 2);
        takeOrder("Ahn Sung Tang Men", 1);
        takeOrder("Ahn Sung Tang Men", 2);
        takeOrder("Ahn Sung Tang Men", 3);
        takeOrder("Ahn Sung Tang Men", 4);
        takeOrder("Zin Ramen", 4);
        takeOrder("Zin Ramen", 5);
        takeOrder("Ahn Sung Tang Men", 3);
        takeOrder("Zin Ramen", 3);

        for (int i = 0; i < ordersCount; i++) {
            ramen[i].serveNoodle(tables[i]);
        }

        System.out.println("\n Total Coffee objects made: "
                + noodleFactory.getTotalNoodleFlavorMade());
    }
}  
```

```java
package DesignPattern.gof_flyweight.sample01;

public interface INoodle {
    public void serveNoodle(NoodleContext noodleContext);
}

package DesignPattern.gof_flyweight.sample01;

public class NoodleContext {
    private final int tableNumber;

    public NoodleContext(int tableNumber){
        this.tableNumber = tableNumber;
    }

    public int getTable(){
        return this.tableNumber;
    }
}

package DesignPattern.gof_flyweight.sample01;

import java.util.HashMap;

public class NoodleFactory {
    private HashMap <String, INoodle> flavors = new HashMap <String, INoodle>();

    public INoodle getNoodleFlavor(String flavorName){
        INoodle noodle = flavors.get(flavorName);

        if(noodle == null){
            noodle = new Ramen(flavorName);
            flavors.put(flavorName, noodle);
        }

        return noodle;
    }

    public int getTotalNoodleFlavorMade(){
        return flavors.size();
    }
}

package DesignPattern.gof_flyweight.sample01;

public class Ramen implements INoodle {

    private final String flavor;


    public Ramen(String flavor){
        this.flavor = flavor;
        System.out.println("Noodle is created!" + flavor);
    }

    public String getFlavor(){
        return this.flavor;
    }

    public void serveNoodle(NoodleContext noodleContext) {
        System.out.println("Serving" + flavor + " to table " + noodleContext.getTable());
    }

}
```