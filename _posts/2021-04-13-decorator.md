---
title:  "Pattern - Decorator"
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

> 실패에 대해 걱정하지 마라. 한번만 제대로 하면 된다.  
>  - 드류 휴스턴 ( 드롭박스 공동 창업자 )

# 의도 

***

객체에 동적으로 새로운 책임을 추가할 수 있게 합니다. 기능을 추가하기 위해서 서브클래스를 생성하는 것보다 더욱 융통성있는 방법을 제공합니다.

# 동기

***

- 개별적인 객체에 새로운 책임을 추가할 필요가 있을 경우를 이야기 하는데, GUI 툴킷에서 모든 사용자 인터페이스 요소에는 필요 없지만, 어떤 사용자 인터페이스 요소에만 스크롤링과 같은 행동이나 테두리와 같은 속성을 추가할 수 있도록 해줄 필요가 있습니다.

- 이와 같은 상황에서 가장 쉬운 방법은 이미 존재하는 클래스를 상속 받고, 또 다른 클래스에서 기존에 사용하던 동일한 클래스를 상속 받아 테두리 속성을 추가하고 이 서브 클래스의 인스턴스에 테두리가 있도록 하는 방법이지만, 이는 테두리을 선택하는 것 자체가 정적입니다. 사용자는 구성요소에 대해서 언제 , 어떻게 테두리로 장식해야할 지 제어할 수 없습니다. 이와 같이 기존의 객체에 대한 기능의 확장이 일어날 때 해당 구성요소를 둘러싸는 것입니다. 이렇게 무엇인가를 감싸는 객체를 장식자라고 합니다.

- Decorater 의 옳지않은 설계 

![](https://keepinmindsh.github.io/lines/assets/img/decorator_badsample.png){: .align-center}

- Decorater 의 올바른 설계 

![](https://keepinmindsh.github.io/lines/assets/img/decorator_sample01.png){: .align-center}

# 활용성

***

![](https://keepinmindsh.github.io/lines/assets/img/decorator.png){: .align-center}

- 동적으로 또한 투명하게, 다시 말해 다른 객체에 영향을 주지 않고 개개의 객체에 새로운 책임을 추가하기 위해서 사용합니다.

- 제거될 수 있는 책임에 대해서 사용됩니다.

- 실제 상속으로 서브클래스를 계속 만드는 방법이 실질적이지 못할 때 사용합니다. 너무 많은 수의 독립된 확장이 가능할 때 모든 조합을 지원하기 위해 이를 상속으로 해결하면 클래스 수가 폭발적으로 많아지게 됩니다. 아니면, 클래스 정의가 숨겨지든가, 그렇지 않더라도 서브클래싱을 할 수 없게 됩니다.

- 장식자 패턴은 객체에 새로운 행동을 추가할 수 있는 가장 효과적인 방법입니다. 장식자를 사용하면 장식자를 객체와 연결하거나 분리하는 작업을 통해 새로운 책임을 추가하거나 삭제하는 일이 런타임에 가능해집니다.

# 항목에 대한 설명 

***

- Component
*Visual Component*
동적으로 추가할 서비스를 가질 가능성이 있는 객체들에 대한 인터페이스

- ConcreteComponent
*TextView*
추가적인 서비스가 실제로 정의되어야 할 필요가 있는 객체

- Decorator
*Component*
Component 객체에 대한 참조자를 관리하면서 Component에 정의된 인터페이스를 만족하도록 인터페이스를 정의

- ConcreteDecorator
*BorderDecorator, ScrollDecorator*
Component에 새롭게 추가할 서비스를 실제로 구현하는 서비스

# 코드 예제

***

```java

package DesignPattern.gof_decorator;

public class Barrack {

    public static void main(String[] args) {
        Upgrade marinUpgrade = new MarineBasicUprade();

        marinUpgrade.upgrade();

        System.out.println("------------");

        Upgrade steampackUpgrade = new SteamPack(marinUpgrade);

        steampackUpgrade.upgrade();

        System.out.println("------------");

        Upgrade longRangeUpgrade = new LongRange(steampackUpgrade);

        longRangeUpgrade.upgrade();

        System.out.println("------------");

        Upgrade speedUpgrade = new Speed(longRangeUpgrade);

        speedUpgrade.upgrade();

        System.out.println("------------");

    }
}

```

![](https://keepinmindsh.github.io/lines/assets/img/decorator_sampe_console.png){: .align-center}

```java

package DesignPattern.gof_decorator;

public abstract class Upgrade {
    public abstract void upgrade();
}
                                    
package DesignPattern.gof_decorator;

public class MarineBasicUprade extends Upgrade {

    @Override
    public void upgrade() {
        System.out.println("공격력이 +1 증가하였습니다.");
    }
}

package DesignPattern.gof_decorator;

public abstract class UpgradeDecorator extends Upgrade {

    protected Upgrade upgrade;

    public UpgradeDecorator(Upgrade upgrade){
        this.upgrade = upgrade;
    }

    public void upgrade(){
        upgrade.upgrade();
    }
}

package DesignPattern.gof_decorator;

public class SteamPack extends UpgradeDecorator {
    public SteamPack(Upgrade upgrade) {
        super(upgrade);
    }

    @Override
    public void upgrade() {
        super.upgrade();

        applySteamPack();
    }

    public void applySteamPack(){
        System.out.println("스팀팩이 적용되었습니다. ");
    }
}

package DesignPattern.gof_decorator;

public class Speed extends UpgradeDecorator {

    public Speed(Upgrade upgrade){
        super(upgrade);
    }

    public void upgrade() {
        super.upgrade();

        applySteamUpgrade();
    }

    public void applySteamUpgrade(){
        System.out.println("속도가 10 만큼 증가하였습니다.");
    }
}

package DesignPattern.gof_decorator;

public class LongRange extends UpgradeDecorator {
    public LongRange(Upgrade upgrade) {
        super(upgrade);
    }

    @Override
    public void upgrade() {
        super.upgrade();

        applyLongRange();
    }

    public void applyLongRange(){
        System.out.println("공격 사거리가 10 증가하였습니다.");
    }
}

```
