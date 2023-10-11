- [[#tailwind-merge?]]
- [[#tailwind-merge install|tailwind-merge install]]
- [[#tailwind-merge example|tailwind-merge example]]
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
pnpm i tailwind-merge
```

## tailwind-merge example 
```typescript
import { twMerge } from 'tailwind-merge'

twMerge('px-2 py-1 bg-red hover:bg-dark-red', 'p-3 bg-[#B91C1C]')
// → 'hover:bg-dark-red p-3 bg-[#B91C1C]'
```


>[tailwind-merge npm](https://www.npmjs.com/package/tailwind-merge) <br/>
>[tailwind-merge git](https://github.com/dcastil/tailwind-merge)