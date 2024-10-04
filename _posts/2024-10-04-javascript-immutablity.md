---
title: "Javascript Immutability"
excerpt: "Javascript Immutability"

categories:
  - Script
tags:
  - Script 
classes: wide
last_modified_at: 2024-10-04T14:34:00-15:35:00
--

# Immutability ( 불변성 )

- 불변성이란 값이나 상태를 변경할 수 없는 것 
  - 불변성의 의미는 메모리 영역에서의 값 변경이 일어나지 않는 것을 이야기 한다
  - 함수형 프로그래밍에서는 순수함수 즉 동일한 매개변수를 넣었을 때 동일한 리턴 값을 출력하는 함수를 의미한다. 
  - 동시에 순수함수는 외부의 값을 변경하는 사이드 이펙트가 일어나지 않는 조건을 지키는 함수를 뜻한다. 

# React의 경우 

리액트의 경우, 상태 관리를 위해서 상태 업데이트를 할 때는 얕은 비교를 하게 되는데, 이는 실제 참조된 값에 대해서만 비교한다는 것을 의미 합니다.   
즉, 리액트 내의 state 에 대한 변화 값을 비교해야하는 경우, 해당 값에 대해서는 불변성에 의해서 변경여부를 확인 할 수 있습니다.     

여기에서 불변성은 기존 객체의 값을 유지하게 하여 사이드 이펙트를 최소화하고, 신규 변경된 객체의 참조 값만을 비교해서 실제 값으로 변경한다.  
따라서 원본 객체의 값을 직접적으로 변경하면 React 내에서는 해당 값의 상태 변화를 인지할 수 없습니다.  

- 효율적인 상태 업데이트 ( 얕은 비교 수행 )
- 사이드 이펙트 방지 및 프로그래밍 구조의 단순성 

## 불변성을 유지하며 코드 작성하기 

```script 
// ... 으로 새로운 list를 만들고, 맨 뒤에 newRegion을 추가한다. 
// newRegions 새롭게 생성된 리스트의 주소가 담기게 된다.
const newRegions = [...regions, newRegion]
```

```script 
const filteredRegions = region.filter(region => user.region === 1);
console.log(filteredRegions);
```

```script 
updatedRegions = regions.map(region => (region.id === 1) ? {...user, name: "철원"} : region)
```

# 자바스크립에서 원시 타입은 

자바 스크립트에서 원시 타입은 값을 변경할 때, 불변성을 잘 지키는데, 이는 기존의 값을 직접 수정하지 않고 새로운 값을 만들어내는 것입니다. 

```script 
let hello1 = "Hello!"
let hello2 = hello1 
hello1 = "Hi"
console.log(hello1, hello2); // Hi, Hello! 
```