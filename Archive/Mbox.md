> [참고자료](https://ko.mobx.js.org/README.html)<br/>

`MobX` counter 예제를 통해 `React`에서의 `MobX`를 사용해보자..
이 글은 `MobX 6`을 기준으로 작성되었습니다.

<br/>

## MobX란?


- 전역 상태 라이브러리이다.
- 모든 상태변화를 일어나는 부분을 런타임에 자동으로 추적해주고, 의존 트리를 만듭니다. 이를 통해 필요한 경우에만 연산이 진행되고 최적화 작업을 할 필요가 없습니다.
- React 뿐만 아니라, Angular, Vue, Flutter, Dart등에서도 사용이 가능하다
- UI 프레임워크 밖에서 애플리케이션 상태를 관리를 할 수 있다. 따라서 코드 분리가 쉽고 다른 곳에서 사용하기 유용하며 무엇보다 쉽게 테스트 할 수 있다.
- MobX의 러닝커브는 낮은편으로 초기에 작성해야하는 코드가 거의 없으며, redux와 달리 state의 불변성도 걱정하지 않아도 된다.

<br/>

## MobX 주요 용어 및 개념

- `observable`: 추적 및 관찰 가능한 state(상태) 정의
- `action`: state를 변경하는 메소드
- `derivation`: state에서 더 이상의 상호작용 없이 파생될 수 있는 모든 것 EX) 사용자 인터페이스 변화, 남은 todo 갯수 등 .

MobX는 다음과 같이 두 종류로 `derivation`을 구분합니다.

1. `computed` : 현재의 `observable state` 에서 `순수 함수`를 사용하여 파생될 수 있는 값
2. `reaction` : state가 변경될 때 자동으로 발생해야 하는 부수효과. Ex) 웹페이지 UI 변경 등

<br/>

### observable

`observable`을 사용하면 `state`를 모두 자동으로 `observable(관찰 가능)`하게 만들 수 있다.
`observable`은 `MobX 6` 기준 `makeObservable`, `makeAutoObservable` 그리고 `observable` 이 세 가지가 있으며, 모두 추적 가능한 상태의 `state`로 만들어준다.

- `makeObservable`: 일반적으로 `makeObservable`은 클래스 구조에서 사용되며, 첫 번째 인수는 `this`이다. annotations 인수는 주석을 각 구성원(member)에 매핑합니다.
- `makeAutoObservable`: `makeObservable`와 거의 비슷하지만, `makeAutoObservable`은 모든 속성(property)을 기본적으로 추론한다는 점에서 `makeObservable`보다 한층 더 업그레이드된 형태이다. 이를 통해 유지관리가 쉬워지는 장점이있지만, super 클래스나 서브클래스에는 사용할 수 없는 단점이 있다.
- `observable`: 모든 객체를 한 번에 `observable`로 지정하는 함수로써 호출할 수도 있다. source 객체가 복제되고 모든 구성원은 `observable`로 지정한다. 이는 `makeAutoObservable`과 유사하다. `observable`로 반환된 객체는 프록시가 됩니다. 즉, 객체에 나중에 추가된 속성도 observable로 지정됩니다.(프록시 사용이 disabled로 설정된 경우는 제외합니다.)

공식 문서에는 `make(Auto)Observable` 사용을 권장하고 있다.

### action

`action`은 `state`를 변경하는 것을 뜻한다. 원칙적으로 `action`은 항상 이벤트에 대한 응답으로 발생한다. 예를 들어 버튼 클릭, 일부 입력 변경, 웹 소켓 메시지 도착 등.
`makeObservable`을 사용하면 `action`을 따로 작성해줘야 하지만, `makeAutoObservable`은 이를 대신해준다. 하지만 대부분 코드 구조 개선 및 성능 이점을 위해 action을 따로 선언해야 한다.

> 코드 구조 개선 및 성능 이점
>
> 1. 트랜잭션(transaction) 내부에서 실행됩니다. 가장 바깥쪽 `action`이 완료될 때까지 `reaction`은 실행되지 않기 때문에, `action` 실행 중에 생성된 중간값 또는 불완전한 값은 `action`이 완료될 때까지 애플리케이션에서 볼 수 없다.
> 2. 기본적으로 `action` 외부에서 state를 변경할 수 없습니다. 이를 통해 코드에서 state 업데이트가 발생하는 위치를 명확히 확인할 수 있다.

### computed

`computed`는 다른 `observable`들에서 어떠한 정보를 도출하는데 사용할 수 있다. 또한, `observables` 중 하나가 변경된 경우에만 다시 계산합니다.
`computed` 값은 Javascript `getters`를 통해 생성할 수 있다.

> computed 사용 시 주의사항
>
> 1. 부수효과(side effects)를 가지거나 다른 observable 항목을 업데이트하면 안 됩니다.
> 2. 새로운 observable 항목을 만들고 반환하면 안 됩니다.

### reaction

`reaction`의 목표는 자동으로 발생하는 부수효과를 모델링 하는 것입니다. `observable state`에 대한 소비자(consumer)를 만들어내거나 무언가 관련된 요소가 바뀔 때 자동적으로 부수효과를 실행하는 데 있습니다.
대표적으로 `autorun`, `reaction`, `when`이 있습니다.

- autorun
  autorun(effect: (reaction) => void)<br/>
  autorun 함수는 변화를 감지할 때마다 실행하는 함수 한 개를 가지며, autorun 자체를 생성할 때도 한 번 실행됩니다. autorun은 observable 또는 computed로 주석 설정한 observable state의 변화에만 반응합니다.
- reaction
  reaction(() => value, (value, previousValue, reaction) => { sideEffect }, options?)<br/>
  reaction은 autorun과 유사하지만 추적할 observable에 대해 보다 세밀하게 제어할 수 있습니다. reaction은 다음과 같이 두 개의 함수를 취합니다. 첫 번째 data 함수는 트래킹 되어 두 번째 effect 함수에 대한 input으로 사용되는 데이터를 반환합니다. 부수효과는 오직 data 함수에서 액세스 된 데이터에만 반응하며, 이는 effect 함수에 실제로 사용되는 데이터보다 적을 수 있다는 점에 유의해야 합니다.

  일반적인 패턴은 data 함수에서 부수 효과에 필요한 항목을 생성하여 effect가 트리거 되는 시점을 보다 정확하게 제어하는 것입니다. 기본적으로 effect 함수가 트리거 되기 위해서는 data 함수의 결과가 변경되어야 합니다. autorun과 달리 부수효과는 초기화될 때 실행되지 않으며, 데이터 표현(expression)이 처음으로 새로운 값을 반환할 때에만 실행됩니다.

- when
  when(predicate: () => boolean, effect?: () => void, options?)<br/>
  when(predicate: () => boolean, options?): Promise<br/>
  when은 true를 반환할 때까지 주어진 predicate 함수를 관찰하고 실행합니다. true가 반환되면 지정된 effect 함수를 실행하고 자동 실행기를 삭제(dispose)합니다.

  when 함수는 disposer를 반환하므로 두 번째 effect 함수를 전달하지 않는 한 수동으로 취소할 수 있으며, 이 경우 Promise가 반환됩니다.

> [참고자료](https://ko.mobx.js.org/reactions.html)

<br/>

## React Mobx 써드파티 라이브러리 및 최근 사항

1. `mobx-react`는 `클래스형 컴포넌트`와 `hooks`를 모두 지원
2. `mobx-react-lite`는 `hooks`만 지원

`MobX`를 사용하려는 프로젝트에서 이미 `hooks`를 사용중이라면, 조금 더 가벼운 `mobx-react-lite` 사용을 권장한다.
또한, `MobX 6`에서 `decorators`(ex. @action, @observable 등)들이 `deprecated` 되었다.

<br/>

## MobX React에 설치하기

```plainText
npx create-react-app my-app
cd my-app

npm install mobx mobx-react --save
```

<br/>

## MobX Store 구축 및 활용

1. `makeObservable`로 Store 만들기
   > src/store/count1.js

```javascript
import { action, makeObservable, observable } from 'mobx';

class Count1 {
  number = 0;

  constructor() {
    makeObservable(this, {
      number: observable,
      increase: action,
      decrease: action,
    });
  }

  increase = () => {
    this.number++;
  };
  decrease = () => {
    this.number--;
  };
}

const countStore1 = new Count1();
export default countStore1;
```

<br/>

2. `makeAutoObservable`로 Store 만들기
   > src/store/count2.js

```javascript
import { makeAutoObservable } from 'mobx';

class Count2 {
  number = 0;

  constructor() {
    makeAutoObservable(this);
  }

  increase = () => {
    this.number++;
  };
  decrease = () => {
    this.number--;
  };
}

const countStore2 = new Count2();
export default countStore2;
```

<br/>

3. `observable`로 Store 만들기
   > src/store/count3.js

```javascript
import { observable } from 'mobx';

const countObject = observable({
  number: 0,
  increase() {
    this.number++;
  },
  decrease() {
    this.number--;
  },
});

export default countObject;
```

<br/>

- 생성한 store 하나의 객체로 만들기
  > src/store/index.js

```javascript
import countStore1 from './count1';
import countStore2 from './count2';
import countStore3 from './count3';

const store = { countStore1, countStore2, countStore3 };

export default store;
```

<br/>

- store 안의 state, action 활용해보기
  > src/app.js

```javascript
import { observer } from 'mobx-react';
import store from './store';

const App = observer(() => {
  const { countStore1, countStore2, countStore3 } = store;

  return (
    <div className="App">
      makeObservable Count: {countStore1.number}{' '}
      <button onClick={() => countStore1.increase()}>+</button>
      <button onClick={() => countStore1.decrease()}>-</button>
      <hr />
      makeAutoObservable Count: {countStore2.number} <button onClick={() => countStore2.increase()}>
        +
      </button>
      <button onClick={() => countStore2.decrease()}>-</button>
      <hr />
      observable Count: {countStore3.number}{' '}
      <button onClick={() => countStore3.increase()}>+</button>
      <button onClick={() => countStore3.decrease()}>-</button>
      <hr />
    </div>
  );
});

export default App;
```

<br/>

- computed 알아보기
  액션(클릭)이 일어날 때마다, 계산된 값이 배가 되는 double을 만들어 보자.

> src/store/double.js

```javascript
import { observable } from 'mobx';

const doubleStore = observable({
  value: 1,
  get double() {
    return this.value * 2;
  },
  increment() {
    this.value++;
  },
});

export default doubleStore;
```

> src/store/index.js

```javascript
import countStore1 from './count1';
import countStore2 from './count2';
import countStore3 from './count3';
import doubleStore from './double';

const store = { countStore1, countStore2, countStore3, doubleStore };

export default store;
```

> src/app.js

```javascript
import { observer } from 'mobx-react';
import { autorun } from 'mobx';
import store from './store';

const App = observer(() => {
  const { countStore1, countStore2, countStore3, doubleStore } = store;

  autorun(() => {
    if (doubleStore.double) {
      console.log('Double' + doubleStore.double);
    }
    if (doubleStore.double === 8) {
      console.log('만약 value가 4라면 0으로 초기화');
      doubleStore.value = 0;
    }
  });

  return (
    <div className="App">
      makeObservable Count: {countStore1.number}{' '}
      <button onClick={() => countStore1.increase()}>+</button>
      <button onClick={() => countStore1.decrease()}>-</button>
      <hr />
      makeAutoObservable Count: {countStore2.number} <button onClick={() => countStore2.increase()}>
        +
      </button>
      <button onClick={() => countStore2.decrease()}>-</button>
      <hr />
      observable Count: {countStore3.number}{' '}
      <button onClick={() => countStore3.increase()}>+</button>
      <button onClick={() => countStore3.decrease()}>-</button>
      <hr />
      makeObservable Double: {doubleStore.value}
      <button onClick={() => doubleStore.increment()}>+</button>
      <hr />
    </div>
  );
});

export default App;
```

> 이때 autorun을 통해 해당 computed 값이 어떻게 바뀌는지 감지할 수 있다.
> 이번 예제에서는 increment를 누를 때마다 double()이라는 getter가 value에 곱하기 2를 한다. 그리고 계산된 값(computed value)가 8에 도달하면 value를 0으로 초기화시킨다.