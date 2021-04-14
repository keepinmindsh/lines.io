---
title:  "Pattern - Chain Of Responsivility"
excerpt: "Gof Design Pattern"

categories:
  - Pattern
tags:
  - Pattern 
classes: wide
last_modified_at: 2019-04-13T08:06:00-05:00
---

> 오늘날의 모든 소프트웨어는 구조적인 완결성 없이 폭력과 수천 만의 노예들로 벽돌을 한 장 한 장씩 올려 만든 피라미드와 같다.  
>  - Alan Kay, 객체지향 프로그래밍의 아버지 

# 의도 

***

메시지를 보내는 객체와 이를 받아 처리하는 객체들 간의 결합도를 없애기 위한 패턴입니다.
하나의 요청에 대한 처리가 반드시 한 객체에서만 되지 않고, 여러 객체에 그 처리 기회를 주려는 것입니다.

# 동기 

***

![](https://keepinmindsh.github.io/lines/assets/img/ChainOfResponsibility2.png){: .align-center}

- 그래픽 사용자 인터페이스에 있는 문맥 감지 도움말 기능을 생각해 봅시다. 사용자가 정보를 선택하면 그 부분에 대한 도움말 정보를 얻을 수 있습니다. 그러나 도움말의 내용은 선택한 주체가 무엇이며, 현재 그 주체의 상황이 어떠한 가에 따라 다를 수 있습니다. 즉, 대화상자 내에 있는 버튼 위젯은 기본 윈도우에 있는 버튼과 내용이 다른 도움말을 제공해야 합니다. 만약, 선택한 주체에 대한 구체적인 도움말이 없다면, 도움말 시스템은 적어도 응용 프로그램이 정의한 일반적인 도움말이라도 제공할 수 있어야 합니다.
- 따라서 도움말 정보는 구체적인 내용부터 일반적인 내용에 까지 내용의 일반성에 따라 구성하는 것이 자연스럽습니다. 게다가, 도움말에 대한 요청은 여러 사용자 인터페이스 객체 중 어느 하나로 처리된다고 보는게 명확합니다. 즉, 도움말 요청이 어떤 상황에서 발생했는지에 따라서, 요청을 받은 객체가 직접 그 요청을 처리하지 않을 수도 있습니다. 어느 정도의 구체적인 도움말이 가능한가에 따라 여러 인터페이스 중 하나가 이 요청을 처리할 수 있습니다.
- 허나, 문제는 궁극적으로는 도움말을 제공해야할 객체가 도움말 요청을 보내는 객체에게 알려져 있지 않다는 것입니다. 즉, 요청을 일으키는 객체는 실제로 자신에게 해당 도움알을 제공하는 객체가 누구인지 알 수 없다는 것입니다. 이를 위해서 우리는 도움말 요청을 발생시키는 버튼과 도움말 정보를 제공하는 객체를 분리해야할 필요가 있습니다. 이 부분의 기작을 정의하는 패턴이 바로 책임 연쇄 패턴입니다.

# 활용성 

***

![](https://keepinmindsh.github.io/lines/assets/img/ChainOfResponsibility.png){: .align-center}

- 하나 이상의 객체가 요청을 처리해야하고, 그 요청 처리자 중 어떤 선이 선행자인지 모를 때, 처리자가 자동으로 확정되어야 한다.
- 메세지를 받을 객체를 명시하지 않은 채 여러 객체 중 하나에게 처리를 요청하고 싶을 때
- 요청을 처리할 수 있는 객체 집합이 동적으로 정의되어야 할 때

# 항목에 대한 설명

***

- Handler  
*Help Handler*  
요청을 처리하는 인터페이스를 정의하고, 후속 처리자와 연결을 구현합니다. 즉, 연결 고리에 연결된 다음 객체에게다시 메세지를 보냅니다.

- ConcreteHandler  
*PrintButton, PrintDialog*  SS
책임 져야할 행동이 있다면 스스로 요청을 처리하여 후속 처리자에 접근할 수 있습니다. 즉, 자신이 처리할 행동이 있으면 처리하고, 그렇지 않으면 후속처리자에 다시 처리를 요청합니다.

# 결과

***

- 장점
  - 객체 간의 행동적 결합도가 작아집니다.

다른 객체가 어떻게 요청을 처리하는지 몰라도 됩니다. 단지 요청을 보내는 객체는 이 메시지가 적절하게 처리될 것이라는 것만 확신하면 됩니다.
객체에 책임을 할당하는 데 유연성을 높일 수 있습니다.
객체의 책임을 여러 객체에게 분산시킬 수 있으므로 런타임에 객체 연결 고리를 변경하거나 추가하여 책임을 변경하거나 확장할 수 있습니다.

- 단점
  - 메시지 수신이 보장되지는 않습니다.

어떤 객체가 이 처리에 대한 수신을 담당한다는 것을 명시하지 않으므로 요청이 처리된다는 보장은 없습니다.

# 코드 예제 -- 기본 샘플 

***

```java

package DesignPattern.gof_chainofresponsibility.sample01;

import DesignPattern.gof_chainofresponsibility.sample01.Teran.Bunker;
import DesignPattern.gof_chainofresponsibility.sample01.Teran.Defense;
import DesignPattern.gof_chainofresponsibility.sample01.Teran.MissileTurret;
import DesignPattern.gof_chainofresponsibility.sample01.Unit.AirUnit;
import DesignPattern.gof_chainofresponsibility.sample01.Unit.GroundUnit;
import DesignPattern.gof_chainofresponsibility.sample01.Unit.Unit;

import java.util.Arrays;
import java.util.List;
import java.util.stream.Stream;

public class BattleGround {

    public static void main(String[] args) {
        Defense defenseBunker = new Bunker("Bunker");
        Defense defenseMissileTurret = new MissileTurret("Missile Turret");

        defenseBunker.setDifense(defenseMissileTurret);

        List<Unit> unitList = Arrays.asList(new AirUnit("커세어"), new GroundUnit("마린"), new AirUnit("배틀쉽"));

        Stream<Unit> streamUnit = unitList.stream();

        streamUnit.forEach(unit -> {
            defenseBunker.attackByUnit(unit);
        });

    }
}

```

```java

// Unit
package DesignPattern.gof_chainofresponsibility.sample01.Unit;

public interface Unit {
    public void Attack();
}

// Ground Unit
package DesignPattern.gof_chainofresponsibility.sample01.Unit;

public class GroundUnit implements Unit {

    String unitName;

    public GroundUnit(String unitName){
        this.unitName = unitName;
    }

    public void Attack() {
        System.out.println("지상 유닛이 공격합니다.");
    }

    public String toString(){
        return unitName;
    }
}

// Air Unit
package DesignPattern.gof_chainofresponsibility.sample01.Unit;

public class AirUnit implements Unit {

    String unitName;

    public AirUnit(String unitNmae){
        this.unitName = unitNmae;
    }

    public void Attack() {
        System.out.println("공중 유닛이 공격합니다.");
    }

    public String toString(){
        return unitName;
    }
}

// Defense
package DesignPattern.gof_chainofresponsibility.sample01.Teran;

import DesignPattern.gof_chainofresponsibility.sample01.Unit.Unit;

public abstract class Defense {

    protected Defense defense;

    public Defense setDifense(Defense defense){
        this.defense = defense;
        return defense;
    }

    public final void attackByUnit(Unit unit){
        System.out.println(unit.toString() + " ------------------- Battle Start -------------------");
        System.out.println();
        if(defense(unit)) {
            System.out.println(unit.toString() + " 의 방어에 성공했습니다.");
            System.out.println();
            System.out.println(unit.toString() +  " -------------------- Battle End ---------------------");
        }else if(defense != null) {
            defense.attackByUnit(unit);
        }else {
            System.out.println(unit.toString() + " 의 방어에 실패했습니다.");
            System.out.println();
            System.out.println(unit.toString() +  " -------------------- Battle End ---------------------");
        }

    }

    public abstract boolean defense(Unit unit);

    public abstract boolean tryToDefense(Unit unit);

    public abstract String getBuildingName();
}

// Bunker
package DesignPattern.gof_chainofresponsibility.sample01.Teran;

import DesignPattern.gof_chainofresponsibility.sample01.Unit.GroundUnit;
import DesignPattern.gof_chainofresponsibility.sample01.Unit.Unit;

public class Bunker extends Defense {

    private final String unitName;

    public Bunker(String unitName){
        this.unitName = unitName;
    }

    @Override
    public boolean defense(Unit unit) {
        System.out.println("----------------- Bunker --------------");

        boolean isSurvive =  tryToDefense(unit);

        if(isSurvive){
            System.out.println("적의 공격을 방어했습니다.");
        }else{
            System.out.println(getBuildingName() + "가 파괴되었습니다.");
        }

        return isSurvive;
    }

    @Override
    public boolean tryToDefense(Unit unit) {
        if(unit instanceof GroundUnit){
            return true;
        }else{
            return false;
        }
    }

    @Override
    public String getBuildingName() {
        return unitName;
    }
}

// MissileTurret
package DesignPattern.gof_chainofresponsibility.sample01.Teran;

import DesignPattern.gof_chainofresponsibility.sample01.Unit.AirUnit;
import DesignPattern.gof_chainofresponsibility.sample01.Unit.Unit;

public class MissileTurret extends  Defense{

    private final String unitName;

    public MissileTurret(String unitName){
        this.unitName = unitName;
    }

    @Override
    public boolean defense(Unit unit) {
        System.out.println("----------------- Missile Turret --------------");

        boolean isSurvive =  tryToDefense(unit);

        if(isSurvive){
            System.out.println("적의 공격을 방어했습니다.");
        }else{
            System.out.println(getBuildingName() + "가 파괴되었습니다.");
        }

        return isSurvive;
    }

    @Override
    public boolean tryToDefense(Unit unit) {
        if(unit instanceof AirUnit){
            return true;
        }else{
            return false;
        }
    }

    @Override
    public String getBuildingName() {
        return unitName;
    }
}


```

```shell

오전 11:09:28: Executing task 'BattleGround.main()'...

Connected to the target VM, address: '127.0.0.1:3561', transport: 'socket'
:compileJava UP-TO-DATE
:processResources UP-TO-DATE
:classes UP-TO-DATE
Connected to the VM started by ':BattleGround.main()' (localhost:3577). Open the debugger session tab
:BattleGround.main()
커세어 ------------------- Battle Start -------------------

----------------- Bunker --------------
Bunker가 파괴되었습니다.
커세어 ------------------- Battle Start -------------------

----------------- Missile Turret --------------
적의 공격을 방어했습니다.
커세어 의 방어에 성공했습니다.

커세어 -------------------- Battle End ---------------------
마린 ------------------- Battle Start -------------------

----------------- Bunker --------------
적의 공격을 방어했습니다.
마린 의 방어에 성공했습니다.

마린 -------------------- Battle End ---------------------
배틀쉽 ------------------- Battle Start -------------------

----------------- Bunker --------------
Bunker가 파괴되었습니다.
배틀쉽 ------------------- Battle Start -------------------

----------------- Missile Turret --------------
적의 공격을 방어했습니다.
배틀쉽 의 방어에 성공했습니다.

배틀쉽 -------------------- Battle End ---------------------

BUILD SUCCESSFUL in 1m 13s
3 actionable tasks: 1 executed, 2 up-to-date
오전 11:10:42: Task execution finished 'BattleGround.main()'.
Disconnected from the target VM, address: '127.0.0.1:3561', transport: 'socket'
        

```