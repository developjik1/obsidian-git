## Polymorphism Component 란?
![[Pasted image 20231012112351.png]]

**Polymorphism**은 한국어로 `다형성`이라고 부르는데, `여러 개의 형태를 가진다`라는 의미이다. 

`Polymorphism Component` 란 `다양한 형태의 UI를 나타낼수있는 컴포넌트` 이다.

즉, **무엇이든지 될 수 있는 컴포넌트** 다.

`Polymorphism Component` 는 `가장 추상화된 형태의 Component` 라 할 수 있다.


MUI or  [Box](https://mui.com/material-ui/react-box/) 컴포넌트나 Mantine의 [Box](https://mantine.dev/core/box/) 컴포넌트를 예시로 들 수 있다. 두 UI 라이브러리는 Box라는 Polymorphic한 컴포넌트를 이용하여 재사용성을 높이고 다양한 컴포넌트를 확장성 있게 구현하고 있다. 굉장히 유용한 컴포넌트기 때문에 필자가 재직 중인 회사에서 만들고 사용하는 [디자인 시스템](https://github.com/cobaltinc/co-design)에도 View 컴포넌트를 구현하여 비슷하게 사용하고 있다.