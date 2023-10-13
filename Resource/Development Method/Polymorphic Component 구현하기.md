---
tags:
  - component
---
## Polymorphism 란 ?
**Polymorphism**은 한국어로 `다형성`이라고 부르는데, `여러 개의 형태를 가진다`라는 의미이다. 
## Polymorphism Component 란?
![[Pasted image 20231012112351.png]]

`Polymorphism Component` 란 `다양한 형태의 UI를 나타낼수있는 컴포넌트` 이다.

즉, **무엇이든지 될 수 있는 컴포넌트** 다.

`Polymorphism Component` 는 `가장 추상화된 형태의 Component` 라 할 수 있다.
이를 통해, `재사용성`을 높이고 다양한 Component 를 `확장성` 있게 구현 할 수 있다.

## 대표적인 Polymorphism Component

- MUI  [Box](https://mui.com/material-ui/react-box/) Component
- Chakra UI [Box](https://chakra-ui.com/docs/components/box) Component

## Polymorphism Component Javascript Version

```jsx
/*
Polymorphism Component Box
*/

export const Box = forwardRef(({ as, ...props }, ref) => {
  const Element = as || "div";
  
  return <Element ref={ref} {...props} />;
});
```

```jsx
/**
 * Button.jsx
 */
import Box from './Box';

export const Button = ({ as, ...props }) => {
  return (
    // 위에서 만들어둔 View 컴포넌트를 이용했다.
    <Box as={as || 'button'}
      style={{ backgroundColor: 'black', color: 'white' }} 
      {...props} 
    />
  );
}

/**
 * App.jsx
 */
import { Button } from './Button';

const App = () => {
  return (
    <div>
      // 마치 a 태그처럼 사용할 수 있다.
      <Button as="a" href="https://kciter.so">Click Me!</Button>
    </div>
  );
}
```

## Polymorphism Component TypeScript Version

```tsx

type AsProp<T extends React.ElementType> = {
  as?: T;
};

export type PolymorphicRef<T extends React.ElementType> =
  React.ComponentPropsWithRef<T>["ref"];

export type PolymorphicComponentProps<
  T extends React.ElementType,
  Props = {}
> = AsProp<T> & React.ComponentPropsWithoutRef<T> & Props & {
  ref?: PolymorphicRef<T>;
};
```

```ts
/*
Polymorphism Component Box
*/

export type BoxProps<T extends React.ElementType> = PolymorphicComponentProps<T>;

type BoxComponent = <T extends React.ElementType = "div">(
  props: BoxProps<T>
) => React.ReactElement | null;

export const Box: BoxComponent = forwardRef(
  <T extends React.ElementType = "div">(
    { as, ...props }: BoxProps<T>,
    ref: PolymorphicRef<T>["ref"]
  ) => {
    const Element = as || "div";
    // size와 color를 style로 적용
    return <Element ref={ref} {...props} style={{ fontSize: size, color }} />;
  }
);
```

```ts
/*
Polymorphism Component Text
*/

type _TextProps = {
  size: number;
  color: string;
};

export type TextProps<T extends React.ElementType> = 
  PolymorphicComponentProps<T, _TextProps>;

type TextComponent = <T extends React.ElementType = "span">(
  props: TextProps<T>
) => React.ReactElement | null;

export const Text: TextComponent = forwardRef(
  <T extends React.ElementType = "span">(
    { as, size, color, ...props }: TextProps<T>,
    ref: PolymorphicRef<T>["ref"]
  ) => {
    const Element = as || "span";
    // size와 color를 style로 적용
    return <Element ref={ref} {...props} style={{ fontSize: size, color }} />;
  }
);
```