# tailwind-merge 란?
- 

```shell
npm i tailwind-merge
```


```typescript

import { type ClassValue, clsx } from "clsx"  
import { twMerge } from "tailwind-merge"  

 export function cn(...inputs: ClassValue[]) {  
  return twMerge(clsx(inputs))  
}

```
