
## tailwind-merge?
Utility function to efficiently merge [Tailwind CSS](https://tailwindcss.com/) classes in JS without style conflicts.

## tailwind-merge install
```shell
npm i tailwind-merge
```

```shell
yarn add tailwind-merge
```

```shell
pnpm add tailwind-merge
```

## tailwind-merge example 
```typescript
import { twMerge } from 'tailwind-merge'

twMerge('px-2 py-1 bg-red hover:bg-dark-red', 'p-3 bg-[#B91C1C]')
// → 'hover:bg-dark-red p-3 bg-[#B91C1C]'
```

## tailwind-merge feature
기본적으로 결과값은 `캐싱`되기 때문에 불필요한 `리렌더링`을 걱정하지 않아도 됩니다.

이 라이브러리는 내부적으로 `LRU cache` 알고리즘을 사용하고 있고

캐시 사이즈를 만약 수정하고 싶다면 extendTailwindMerge를 사용하면 됩니다.

캐시힛 없이 twMerge호출이 빠르게 유지될 수 있도록 비싼 계산은 upfront로 수행한다고하네요

이부분은 구현에 가까운 내용이니 일단은 이정도만 알고 넘어가도록 합시다.

이 tailwind-merge가 제공하는 강력한 기능은 다음과 같습니다.

실제로는 더 많은 기능을 제공해주고 있지만

당장 사용하고 싶은 사람의 입장에서 크게 눈에 띄는 기능은 이것입니다.

|   |   |
|---|---|
|Last Conflicting Class Wins|이름에서 눈치 챌 수 있듯이 맨 마지막에 적용시킨 클래스가  <br>win , 적용된다는 뜻입니다.|

>[tailwind-merge npm](https://www.npmjs.com/package/tailwind-merge)   
>[tailwind-merge git](https://github.com/dcastil/tailwind-merge)