# Humble Style Guide

## Javascript/Typescript

Стайлгайд основывается на [typescript-book](https://github.com/basarat/typescript-book/blob/master/docs/styleguide/styleguide.md)

Константы только в верхнем регистре
```js
// Bad
const defaultCode = 1;
const defaultCodes = { moscow: 1 };

// Good
const DEFAULT_CODE = 1;
const DEFAULT_CODES = { MOSCOW: 1 };
```

Булевые переменные должны иметь перфикс is, has, can, should, was

```js
// Bad
const disabled = true;
const children = false;
const collapse = true;

// Good
const isDisabled = true;
const hasChildren = false;
const canCollapse = true;
```

Не использовать any
```js
// Bad
let basket: any[] = [];

// Good
let basket: (BasketFoo | BasketBar)[] = [];
```

Alias начинаются с символа @
```js
// Bad
const alias = {
    src: path.resolve(__dirname, '../../src'),
}

// Good
const alias = {
    '@src': path.resolve(__dirname, '../../src'),
}
```

Обязательное использование alias в импортах
```js
// Bad
import { Foo }  from '../../Foo';
import { Bar } from '../../dir/Bar';

// Good
import { Foo }  from '@src/Foo';
import { Bar } from '@dir/Bar'; // or @src/dir/Bar
```

Порядок импортов в файле
 - импорты из node_modules
 - импорты по абсолютному пути
 - импорты по относительному пути
```js
// Bad
import { FooType } from '../../types';
import { BarType } from './types';
import { useEffect } from 'react';

// Good
import { useEffect } from 'react';
import { FooType } from '@src/types';
import { BarType } from './types';
```

## React

Все компоненты должны иметь индекс файл
```
-- Component
  -- index.ts // requred file
  -- Component.tsx
  -- types.ts
  -- styles.ts
```
```js
// content of index.ts file
export * from './Component.tsx'
export * from './types.ts' // of necessity
```

Разбиваем части компонента (типы, стили) на отдельные файлы
```
// Bad
-- Component
  -- index.ts
  -- Component.tsx
  
// Good
-- Component
  -- index.ts
  -- Component.tsx
  -- types.ts
  -- styles.ts
```
```js
// Bad
type ComponentProps = { ... }
type ComponentState = { ... }

const ComponentStyles = css`...`;

export const Component: React.FC<ComponentProps, ComponentsState> = (props) => { ... };

// Good
import { ComponentProps,  ComponentState} from './types.ts';
import { ComponentStyles } from './styles.ts';

export const Component: React.FC<ComponentProps, ComponentsState> = (props) => { ... };
```

Все дочернее компоненты хранятся в директории components
```
// Bad
-- Component
  -- index.ts
  -- Component.tsx
  -- ComponentItem.tsx
  
// Good
-- Component
  -- components
    -- ComponentItem
  -- index.ts
  -- Component.tsx
```

Избегать Prop Drilling (используем контекст если нужно глубоко прокинуть пропсы)
```js
// Bad
const Button = ({ theme }) => {
    // ...some action with the theme
};

const Buttons = ({ theme }) => {
  // ...some buttons render with theme prop  
};

const App = () => {
    const theme = 'dark'
  
    return (
        <Buttons theme={theme} />
    )
};

// Good
const Button = ({ theme }) => {
    const theme = useContext(Context);

    // ...some action with the theme
};

const Buttons = () => {
  // ...some buttons render
};

const App = () => {
    const theme = 'dark'

    return (
        <Context.Provider value={theme}>
            <Buttons />
        </Context.Provider>
    );
};
```

## Redux

Разделять actions, middleware, reducers and etc.. на отдельные файлы
```
// Bad
-- store
  -- model
    -- index.ts
    -- model.ts
  
// Good
-- store
  -- model
    -- api.ts
    -- index.ts
    -- middleware.ts
    -- model.ts
    -- types.ts
    -- reducers.ts
    -- ...
```

Все селекторы имеет префикс select
```js
// Bad
export const getFoo = createSelector(rootSelector, s => s.foo);
export const isBar = createSelector(rootSelector, s => s.bar);

// Good
export const selectFoo = createSelector(rootSelector, s => s.foo);
export const selectBar = createSelector(rootSelector, s => s.bar);
```
