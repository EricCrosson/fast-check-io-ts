# fast-check-io-ts

[io-ts](https://github.com/gcanti/io-ts) codecs mapped to [fast-check](https://github.com/dubzzz/fast-check) arbitraries.

## Usage

```ts
import * as fc from 'fast-check';
import * as t from 'io-ts';
import { getArbitrary } from 'fast-check-io-ts';

const NonEmptyString = t.brand(
  t.string,
  (s): s is t.Branded<string, { readonly NonEmptyString: symbol }> => s.length > 0,
  'NonEmptyString'
);
const User = t.type({
  name: t.string,
  status: t.union([t.literal('active'), t.literal('inactive')]),
  handle: NonEmptyString
});

const userArb = getArbitrary(User);

console.log(fc.sample(userArb, 1)[0]); // { name: '', status: 'inactive', handle: 'M.y?>A/' }
```

## Known issues

- only supports predefined `io-ts` codecs, not custom types e.g. `DateFromISOString` from `io-ts-types`
- `getArbitrary(t.keyof({ ... }))` is currently marked as error by TS, even though the codec is supported in the implementation
