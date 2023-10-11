## class-variance-authority? 
A tiny (234B) utility for constructing `className` strings conditionally.  
Also serves as a [faster](https://github.com/lukeed/clsx/blob/HEAD/bench) & smaller drop-in replacement for the `classnames` module.

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

## clsx example
```typescript
import clsx from 'clsx';
// or
import { clsx } from 'clsx';

// Strings (variadic)
clsx('foo', true && 'bar', 'baz');
//=> 'foo bar baz'

// Objects
clsx({ foo:true, bar:false, baz:isTrue() });
//=> 'foo baz'

// Objects (variadic)
clsx({ foo:true }, { bar:false }, null, { '--foobar':'hello' });
//=> 'foo --foobar'

// Arrays
clsx(['foo', 0, false, 'bar']);
//=> 'foo bar'

// Arrays (variadic)
clsx(['foo'], ['', 0, false, 'bar'], [['baz', [['hello'], 'there']]]);
//=> 'foo bar baz hello there'

// Kitchen sink (with nesting)
clsx('foo', [1 && 'bar', { baz:false, bat:null }, ['hello', ['world']]], 'cya');
//=> 'foo bar hello world cya'
```

## clsx feature
- 조건부 렌더를 작성하기 쉬워진다.

## clsx typescript