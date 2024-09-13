# React zustand

## Installation

```bash
yarn add zustand
```

## Usage

Use state function in a nested props to avoid re rendering when consuming the root store.

> Simple store

```ts
import { create } from "zustand";

type CountState = {
  count: number;
  actions: {
    setCount: (count: number) => void;
  };
};

export const useCountStore = create<CountState>()((set) => ({
  count: 0,
  actions: {
    setCount: (count) => set((state) => ({ count })),
  },
}));
```

> Local storage store

```ts
import { create } from "zustand";
import { persist } from "zustand/middleware";

type CountState = {
  count: number;
  actions: {
    setCount: (count: number) => void;
  };
};

// This store is persisted in localStorage
export const useCountStore = create<CountState>()(
  persist(
    (set) => ({
      count: 0,
      actions: {
        setCount: (count) => set((state) => ({ count })),
      },
    }),
    {
      name: "my-store",
      partialize: (state) => ({ count: state.count }), // to only store needed props
    }
  )
);
```
