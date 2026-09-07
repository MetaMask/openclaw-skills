---
repo: metamask-mobile
parent: selector-patterns
---

# Selector patterns — MetaMask Mobile

Copy from `app/selectors/addressBookController.ts` (leaf + `createSelector` + `createDeepEqualSelector`) and `app/selectors/tokensController.ts` (collection outputs, stable empty constant). Tests: `yarn jest app/selectors/<feature>.test.ts --no-coverage`. Unit-test shape: `mobile-testing`. Engine wiring that needs a first selector: install/use `coding/selector-patterns` from `controller-integration`. Memoization audits: `performance`. Version-gated flags: `feature-flags`.

## Paths

| Role | Path |
|------|------|
| Default selectors | `app/selectors/<feature>.ts` (or `app/selectors/<feature>/index.ts`) |
| Feature-private selectors | `app/components/UI/<Feature>/selectors/` |
| Deep-equal helper | `app/selectors/util.ts` → `createDeepEqualSelector` |
| Collocated tests | `app/selectors/<feature>.test.ts` |

New selectors use `select<Feature><Thing>` (e.g. `selectTokensByChainId`). Existing `get*` names stay; do not rename them in this change.

## Leaf vs derived

Only leaf input selectors read `state.engine.backgroundState`. Use `?? getDefault<Name>State()` when that helper exists.

```ts
import { RootState } from '../reducers';
import { createSelector } from 'reselect';
import { createDeepEqualSelector } from './util';
import { Hex } from '@metamask/utils';

const EMPTY_ADDRESS_BOOK: Readonly<Record<string, never>> = Object.freeze({});
const EMPTY_ADDRESS_BOOK_CHAIN: readonly never[] = Object.freeze([]);

export const selectAddressBookControllerState = (state: RootState) =>
  state.engine.backgroundState.AddressBookController;

export const selectAddressBook = createSelector(
  selectAddressBookControllerState,
  (addressBookControllerState) =>
    addressBookControllerState?.addressBook ?? EMPTY_ADDRESS_BOOK,
);

export const selectAddressBookByChain = createDeepEqualSelector(
  [selectAddressBook, (_state: RootState, chainId: Hex) => chainId],
  (addressBook, chainId: Hex) => {
    if (!addressBook[chainId]) {
      return EMPTY_ADDRESS_BOOK_CHAIN;
    }
    return Object.values(addressBook[chainId]);
  },
);
```

`EMPTY_ADDRESS_BOOK` / `EMPTY_ADDRESS_BOOK_CHAIN` are module-level constants (see `EMPTY_TOKENS_BY_ADDRESS` in `tokensController.ts`). Inline `?? {}` / `?? []` on a plain `createSelector` allocates a new ref every call.

## Factory choice

| Output | Factory |
|--------|---------|
| boolean, number, string | `createSelector` from `reselect` |
| object, array, Map, Set | `createDeepEqualSelector` from `app/selectors/util.ts` |

Identity passthrough of a controller slice (`createSelector(selectX, (s) => s.things)`) belongs on `createDeepEqualSelector`. Copy before sort: `[...items].sort(cmp)`.

## UI

```ts
const addressBook = useSelector(selectAddressBook);
```

## Tests

Collocate `app/selectors/<feature>.test.ts`. At least two state variants per new selector (populated + empty/missing). Present tense, AAA, no "should". `yarn jest app/selectors/<feature>.test.ts --no-coverage`.

```ts
describe('selectAddressBook', () => {
  it('returns addressBook from AddressBookController state', () => {
    const mockState = {
      engine: {
        backgroundState: {
          AddressBookController: {
            addressBook: {
              '0x1': {
                '0x123': {
                  address: '0x123',
                  name: 'Alice',
                  chainId: '0x1',
                  memo: 'Friend',
                  isEns: false,
                },
              },
            },
          },
        },
      },
    };

    expect(selectAddressBook(mockState as RootState)).toEqual({
      '0x1': {
        '0x123': {
          address: '0x123',
          name: 'Alice',
          chainId: '0x1',
          memo: 'Friend',
          isEns: false,
        },
      },
    });
  });

  it('returns an empty object when addressBook is missing', () => {
    const mockState = {
      engine: {
        backgroundState: {
          AddressBookController: {},
        },
      },
    };

    expect(selectAddressBook(mockState as RootState)).toEqual({});
  });
});
```

## Requirements

- New and changed selectors live under `app/selectors/<feature>.ts` (or the feature’s `selectors/` folder), named `select<Feature><Thing>`.
- Only leaf input selectors read `state.engine.backgroundState` (with `?? getDefault<Name>State()` when that helper exists).
- Derive with `createSelector` (primitives) or `createDeepEqualSelector` (object / array). Stable module-level empty constants instead of inline `?? {}` / `?? []` on plain `createSelector`.
- Collocated unit tests with at least two state variants.
- Components and hooks use `useSelector(selectX)` with the named selector.

## Reject

- `state.engine.backgroundState` in components, hooks, or other UI
- Inline `useSelector((state) => …)` derivation
- `useSelector(x, isEqual)` as a substitute for a stable selector
- Identity `createSelector` on a controller slice
- Inline `?? {}` / `?? []` creating a new ref on every call
- Mutating inputs in the result function (`items.sort` without copying)
- Version-gated flag evaluation outside `feature-flags`
