## class-variance-authority? 

CSS-in-TS libraries such as [Stitches(opens in a new tab)](https://stitches.dev/docs/variants) and [Vanilla Extract(opens in a new tab)](https://vanilla-extract.style/documentation/api/style-variants/) are **fantastic** options for building type-safe UI components; taking away all the worries of class names and StyleSheet composition.

…but CSS-in-TS (or CSS-in-JS) isn't for everyone.

You may need full control over your StyleSheet output. Your job might require you to use a framework such as Tailwind CSS. You might just prefer writing your own CSS.

Creating variants with the "traditional" CSS approach can become an arduous task; manually matching classes to props and manually adding types.

`cva` aims to take those pain points away, allowing you to focus on the more fun aspects of UI development.

## class-variance-authority install
```shell
npm i class-variance-authority
```

```shell
yarn add class-variance-authority
```

```shell
pnpm i class-variance-authority
```

> cva 함수 내부에서는 tailwind intellisense가 동작하지 않습니다.
> 따라서, 아래 링크에 나오는 설정을 해야 합니다.
> [cva tailwind intellisense setting](https://cva.style/docs/getting-started/installation)

## class-variance-authority example
### Creating Variants
```javascript
import { cva } from "class-variance-authority";  
  
const button = cva(["font-semibold", "border", "rounded"], {  
  variants: {  
    intent: {  
      primary: [  
        "bg-blue-500",  
        "text-white",  
        "border-transparent",  
        "hover:bg-blue-600",  
      ],  
      // **or**  
      // primary: "bg-blue-500 text-white border-transparent hover:bg-blue-600",      secondary: [  
        "bg-white",  
        "text-gray-800",  
        "border-gray-400",  
        "hover:bg-gray-100",  
      ],  
    },  
    size: {  
      small: ["text-sm", "py-1", "px-2"],  
      medium: ["text-base", "py-2", "px-4"],  
    },  
  },  
  compoundVariants: [  
    {  
      intent: "primary",  
      size: "medium",  
      class: "uppercase",  
      // **or** if you're a React.js user, `className` may feel more consistent:  
      // className: "uppercase"    },  
  ],  
  defaultVariants: {  
    intent: "primary",  
    size: "medium",  
  },  
});  
  
button();  
// => "font-semibold border rounded bg-blue-500 text-white border-transparent hover:bg-blue-600 text-base py-2 px-4 uppercase"  
  
button({ intent: "secondary", size: "small" });  
// => "font-semibold border rounded bg-white text-gray-800 border-gray-400 hover:bg-gray-100 text-sm py-1 px-2"

```

`cva`의 `첫번째` 인자에는 모든 경우에 `공통`으로 들어가게 될 `css`를 입력하게 됩니다.

그리고 `두번째 인자`에는 객체를 넣어줍니다.

>[class-variance-authority npm](https://www.npmjs.com/package/class-variance-authority)   
>[class-variance-authority homepage](https://cva.style/docs/getting-started/variants)