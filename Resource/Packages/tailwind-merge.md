# tailwind-merge 란?
- Utility function to efficiently merge [Tailwind CSS](https://tailwindcss.com/) classes in JS without style conflicts.

# tailwind-merge install
```shell
npm i tailwind-merge
```

```shell
yarn add tailwind-merge
```

```shell
pnpm i tailwind-merge
```

```typescript

import { type ClassValue, clsx } from "clsx"  
import { twMerge } from "tailwind-merge"  

 export function cn(...inputs: ClassValue[]) {  
  return twMerge(clsx(inputs))  
}

```


>[tailwind-merge npm](https://www.npmjs.com/package/tailwind-merge)
>[tailwind-merge git](https://github.com/dcastil/tailwind-merge)