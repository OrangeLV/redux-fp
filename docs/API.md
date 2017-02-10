<!-- Generated by documentation.js. Update this documentation by updating the source code. -->

### Table of Contents

-   [match](#match)
-   [withDefaultState](#withdefaultstate)
-   [concat](#concat)
-   [combine](#combine)
-   [handleAction](#handleaction)
-   [handleActions](#handleactions)
-   [updateStateAt](#updatestateat)
-   [mapState](#mapstate)
-   [filterState](#filterstate)
-   [constantState](#constantstate)
-   [decorate](#decorate)
-   [Updater](#updater)

## match

**Syntax**

```javascript
match(predicateUpdater, leftUpdater)
```

```javascript
match(predicateUpdater, leftUpdater, rightUpdater)
```

```javascript
match(predicateUpdater)(leftUpdater)
```

```javascript
match(predicateUpdater)(leftUpdater, rightUpdater)
```

**Description**

Creates a proxy updater that:

-   calls **`leftUpdater`** if `predicateUpdater` returns true, or
-   calls **`rightUpdater`** if `predicateUpdater` returns false and `rightUpdater` is defined, or
-   returns the state unchanged if `predicateUpdater` returns false and `rightUpdater` is undefined.

**Parameters**

-   `predicateUpdater` **function (action: any): function (state: any): [boolean](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** 
-   `leftUpdater` **[Updater](#updater)** 
-   `rightUpdater` **[Updater](#updater)?** 

**Examples**

```javascript
match(p, t, f)
// Is equivalent to
action => state => p(action)(state) ? t(action)(state) : f(action)(state)
```

```javascript
match(p, t)
// Is equivalent to
action => state => p(action)(state) ? t(action)(state) : state
```

Returns **[Updater](#updater)** 

## withDefaultState

**Syntax**

```javascript
withDefaultState(defaultState, updater)
```

```javascript
withDefaultState(defaultState)(updater)
```

**Description**

Creates a proxy updater that:

calls `updater` with `defaultState` if incoming state is undefined, or calls it with the incoming state.

**Parameters**

-   `defaultState` **any** 
-   `updater` **[Updater](#updater)** 

**Examples**

```javascript
withDefaultState(0, add)
// Is equivalent to
action => (state = 0) => add(action)(state)
```

Returns **[Updater](#updater)** 

## concat

**Syntax**

```javascript
concat(...updaters)
```

**Description**

Creates a proxy updater that:

calls each updater with the preceding updater's outgoing state (left-to-right).

**Parameters**

-   `updaters` **...[Updater](#updater)** 

**Examples**

```javascript
concat(f, g, h)
// Is equivalent to
action => state => h(action)(g(action)(f(action)(state)))
```

Returns **[Updater](#updater)** 

## combine

**Syntax**

```javascript
combine(pathFragmentUpdaterMap)
```

**Description**

Like [`combineReducers`](http://redux.js.org/docs/api/combineReducers.html), but for updaters.

**Parameters**

-   `pathFragmentUpdaterMap` **[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** 

**Examples**

```javascript
combine({
  foo: fooUpdater,
  bar: barUpdater
})
// Is equivalent to
concat(
  updateStateAt('foo', fooUpdater),
  updateStateAt('bar', barUpdater)
)
```

Returns **[Updater](#updater)** 

## handleAction

**Syntax**

```javascript
handleAction(actionType, updater)
```

```javascript
handleAction(actionType)(updater)
```

**Description**

Creates a proxy updater that:

calls `updater` if `actionType` matches incoming action's type, or returns the state unchanged.

**Parameters**

-   `actionType` **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** 
-   `updater` **[Updater](#updater)** 

**Examples**

```javascript
handleAction('SOME_ACTION', someUpdater)
// Is equivalent to
match(action => () => action.type === 'SOME_ACTION', someUpdater)
```

Returns **[Updater](#updater)** 

## handleActions

**Syntax**

```javascript
handleActions(actionTypeUpdaterMap)
```

**Description**

Creates a proxy updater that:

delegates to a matching updater from `actionTypeUpdaterMap` based on the incoming action's type, or returns the state unchanged.

**Parameters**

-   `actionTypeUpdaterMap` **[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** 

**Examples**

```javascript
handleActions({
  ACTION_ONE: updaterOne,
  ACTION_TWO: updaterTwo
})
// Is equivalent to
concat(
  handleAction('ACTION_ONE', updaterOne),
  handleAction('ACTION_TWO', updaterTwo)
)
```

Returns **[Updater](#updater)** 

## updateStateAt

**Syntax**

```javascript
updateStateAt(path, updater)
```

```javascript
updateStateAt(path)(updater)
```

**Description**

Creates a proxy updater that:

calls `updater` with incoming state focused at `path`, then merges the result into outgoing state.

**Parameters**

-   `path` **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** 
-   `updater` **[Updater](#updater)** 

**Examples**

```javascript
const updater = updateStateAt('a.b', () => state => state + 1)
const state = { a: { b: 1 }, c: 0 }

updater()(state)
// Result: `{ a: { b: 2 }, c: 0 }`
```

Returns **[Updater](#updater)** 

## mapState

**Syntax**

```javascript
mapState(updater)
```

**Description**

Creates a proxy updater that:

maps incoming state via `updater` as iteratee.

**Parameters**

-   `updater` **[Updater](#updater)** 

**Examples**

```javascript
mapState(action => state => state + action)
// Is equivalent to
action => state => state.map(item => item + action)
```

Returns **[Updater](#updater)** 

## filterState

**Syntax**

```javascript
filterState(updater)
```

**Description**

Creates a proxy updater that:

filters incoming state via `updater` as predicate.

**Parameters**

-   `updater` **function (action: any): function (state: any): [boolean](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** 

**Examples**

```javascript
filterState(action => state => action > state)
// Is equivalent to
action => state => state.filter(item => action > item)
```

Returns **[Updater](#updater)** 

## constantState

**Syntax**

```javascript
constantState(value)
```

**Description**

Creates an updater that always returns `value`.

**Parameters**

-   `value` **any** 

**Examples**

```javascript
constantState('foo')
// Is equivalent to
() => () => 'foo'
```

Returns **[Updater](#updater)** 

## decorate

**Syntax**

```javascript
decorate(...fns, value)
```

**Description**

Applies to `value` a function created by composing `...fns`.

**Parameters**

-   `fns` **...[Function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function)** 
-   `value` **any** 

**Examples**

```javascript
decorate(f, g, h, value)
// Is equivalent to
f(g(h(value)))
```

```javascript
decorate(
  withDefaultState(0),
  handleAction('ADD'),
  action => state => state + action.payload
)
// Is equivalent to
withDefaultState(0, handleAction('ADD', action => state => state + action.payload))
```

Returns **any** 

## Updater

Action-first curried reducer.

Type: function (action: any): function (state: any): any