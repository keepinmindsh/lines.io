---
title:  "Pattern - Composite"
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

> 리더십이란, 리더가 없는 상황에서도 그 영향력이 지속되도록 하는 것
>   - 셰릴 샌드버그 ( 페이스북 CEO )

# 의도 

***

부분과 전체의 계층을 표현하기 위해 객체들을 모아 트리 구조로 구성합니다. 사용자로 하여금 개별 객체와 복합 객체를 모두 동일하게 다룰 수 있도록 하는 패턴입니다.

# 동기 

***

그래픽 편집기나 구조도 캡쳐 시스템 같은 그래픽 응용 프로그램을 살펴보면 사용자가 간단한 그림 구성 요소들을 모아서 복잡한 다이어그램을 생성할 수 있게 합니다. 사용자는 더 큰 그림 요소를 만들기 위해 구성요소들을 그룹으로 만들고, 이 그룹이 다시 더 큰 그룹을 형성하기도 합니다. 이를 구현하기 위한 간단한 방법은 텍스트, 라인 등 간단한 기본 그림 요소에 대한 클래스들을 정의하고, 이를 포함한 컨테이너로 동작하는 클래스를 추가로 정의하는 것입니다. 하지만 이 접근법에는 문제가 있습니다. 각각의 그룹에 해당하는 클래스들과 컨테이너 클래스를 구분하기 위해 클래스의 속성으로 크기, 색깔, 위치 등의 기본 속성 이외의 코드에 해당하는 속성을 정의해야 합니다. 또한 이를 사용하는 응용 프로그램 개발자는 코드 값을 기억하고 코드에 따라서 여러 분기를 처리해야 하는 어려움이 있습니다.

- 잘못된 구성의 설계 

![](https://keepinmindsh.github.io/lines/assets/img/composit_wrong.png){: .align-center}

그래픽 편집기의 경우 Line, Rectangle, Text, Picture 등의 다양한 구성요소가 존재하고, 이를 하나의 컨테이너에 담아서 처리할 수 있습니다. 하지만 각 구성요소 별로 크기,색깔, 위치 등의 기본속성이 다를 것이고, 이를 그래픽 편집기에서 쓰기위해서는 각 구성요소 별로의 속성 값에 따라서 분기 처리해서 사용해야하고 이는 개발자의 기억에 의존하는 문제가 발생할 수 있습니다.


# 활용성

***

![](https://keepinmindsh.github.io/lines/assets/img/composite_graphic.png){: .align-center}

- 복합체 패턴의 가장 중요한 요소는 기본 클래스와 이들의 컨테이너를 모두 표현할 수 있는 하나의 추상화 클래스를 정의하는 것입니다. 그래픽 응용 프로그램 예에서 추상 클래스로 그래픽 편집기를 정의하였을 때, 그래픽 편집기는 그림을 그리기위한 기본 연산인 Draw() 뿐만이 아니라, 이런 기본 클래스를 포함하고 관리하는데 필요한 Add(), Remove(), GetChild() 등의 연산도 정의되어 있습니다.

- 복합 객체의 경우, 하위와 상위 계층이 존재할 수 있습니다. 이 때문에 재귀적인 특성으로 이를 구현할 수 있으며, 상위 요소에서 하위요소의 각 객체의 연산을 호출하여 사용할 수 있게 구성합니다. 상위 계층이라는 것은 하위 계층을 묶어 하나로 구성하는 객체(클래스)라고 생각해도 좋습니다. 이 때문에 그림판이나 파워 포인트 등을 쉬운 예제로 많이 들수 있습니다.

  - 부분 - 객체의 객체 계통을 표현하고 싶을 때,
  - 사용자가 객체의 합성으로 생긴 복합 객체와 개개의 객체 사이의 차이를 알지 않고도 자기 일을 할 수 있도록 만들고 싶을 때, 사용자는 복합 구조의 모든 객체를 똑같이 취급하게 됩니다.
  - 복합체 패턴의 주요 목표 중 하나는 사용자가 어떤 Leaf나 Composite 클래스가 존재하는지 모르도록 하고자 할 때,

# 항목에 대한 설명

***

- Component
*Graphic*
집합 관계에 정의될 모든 객체에 대한 인터페이스를 정의합니다. 모든 클래스에 해당하는 인터페이스에 대해서는 공통의 행동을 구현합니다. 전체 클래스에 속한 요소들을 관리하는 데 필요한 인터페이스를 정의합니다. 순환 구조에서 요소들을 포함하는 전체 클래스로 접근하는 데 필요한 인터페이스를 정의하며, 적절하다면 그 인터페이스를 구현합니다.

- Leaf
*Rectangle, Line, Text, 기타*
가장 말단의 객체, 즉 자식(여기서는 포함된 것을 일컫습니다.)이 없는 객체를 나타냅니다. 객체 합성에 가장 기본이 되는 객체의 행동을 정의합니다.

- Composite
*Picture*
자식이 있는 구성요소에 대한 행동을 정의합니다. 자신이 복합하는 요소들을 저장하면서, Component 인터페이스에 정의된 자식 관련 연산을 구현합니다.

- Client
Component 인터페이스를 통해 복합 구조 내의 객체들을 조작합니다.

# 코드 예제 - 기본

***

```java
package DesignPattern.gof_composite.sample002;

public class Client {

    public static void main(String[] args) {

        // 각각의 세부 객체 - 각각의 역할에 따라 다르게 처리됨
        Component leaf1 = new Leaf1();
        Component leaf2 = new Leaf2();
        Component leaf3 = new Leaf3();
        Component leaf4 = new Leaf4();

        Component composite = new Composite();

        // 복합체에 각각의 고유 프로세스 객체를 담는다.
        composite.Add(leaf1);
        composite.Add(leaf2);
        composite.Add(leaf3);
        composite.Add(leaf4);

        // 복합체의 실행
        composite.Operation();
    }
}
```

```java
package DesignPattern.gof_composite.sample002;

import java.util.ArrayList;
import java.util.List;

public class Component {

    public List<Component> children= new ArrayList<>();

    public void Operation(){
        for(Component component : children){
            component.Operation();
        }
    }

    public void Add(Component component){
        children.add(component);
    }

    public void Remove(Component component){
        children.remove(component);
    }

    public void GetChild(int index){
        children.get(index);
    }

}  

package DesignPattern.gof_composite.sample002;

import java.util.ArrayList;
import java.util.List;

public class Composite extends Component {
    public List<Component> children= new ArrayList<>();

    public void Operation(){
        for(Component component : children){
            component.Operation();
        }
    }

    public void Add(Component component){
        children.add(component);
    }

    public void Remove(Component component){
        children.remove(component);
    }

    public void GetChild(int index){
        children.get(index);
    }
}

package DesignPattern.gof_composite.sample002;

public class Leaf1 extends Component {

    @Override
    public void Operation(){
        System.out.println("실질적인 프로세스 처리 : Leaf1");
    }
}

package DesignPattern.gof_composite.sample002;

public class Leaf2 extends Component {

    @Override
    public void Operation(){
        System.out.println("실질적인 프로세스 처리 : Leaf2");
    }
}

package DesignPattern.gof_composite.sample002;

public class Leaf3 extends Component {

    @Override
    public void Operation(){
        System.out.println("실질적인 프로세스 처리 : Leaf3");
    }
}

package DesignPattern.gof_composite.sample002;

public class Leaf4 extends Component {

    @Override
    public void Operation(){
        System.out.println("실질적인 프로세스 처리 : Leaf4");
    }
}
```


