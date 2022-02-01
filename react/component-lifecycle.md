# Component Lifecycle

## Reconciliation

Reconciliation occurs during the update phase.

Updating a component's state marks the component as stale.

After all of the state changes have been applied, React renders the component and all it's child components.

When React renders a component it caches the content that was rendered.  After the component's state is updated, React generates the new content, compares the cached content with the new content and **ONLY** updates the DOM with the part of the content that has changed.

### List Reconciliation

Pass a `key` prop to uniquely identify the element so React can reuse the element.

```jsx
<div>
    { ["Mark", "Evan", "Crystal", "Jocelyn"].map((name, index) => {
        return <p key={name}>{name}</p>
    }) }
<div>
```

### Force Reconciliation

Call the component's `forceUpdate()` method.

This is a blunt force method that should be avoided if possible.

Prefer triggering reconciliation by updating a component's state or through composition.

```jsx
handleClick = () => this.forceUpdate();
```

## Lifecycle (Class Component)

### Mounting Phase

#### `constructor()`

Receive props, initialize state and perform other prepatory work.

#### `getDerivedStateFromProps(props, state)`

Static method.  Unable to access `state` or `props` via `this` keyword.

Return a new `state` object.

#### `render()`

Generate the content that will be rendered in the DOM.

#### `componentDidMount()`

Typically used to execute Ajax requests  

### Update Phase

#### `getDerivedStateFromProps(props, state)`

See above.

#### `shouldComponentUpdate(newProps, newState)`

Use the `newProps` and `newState` parameters to determine if the component should execute it's `render()` method.

Return `false` if the `render()` method should **NOT** be executed.

#### `render()`

See above.

#### `componentDidUpdate()`

Typically used to manipulate HTML elements in the DOM using the React refs feature.

### Unmounting Phase

#### `componentWillUnmount()`

Provides components an opportunity to release resources, close network connections and stop asychronous tasks.

## Lifecycle (Function Component)

### `useEffect()`

Provides a hook that is roughly equivalent to `componentDidMount()`, `componentDidUpdate()` and `componentWillUnmount()`.

```jsx
export function FunctionalComponent() {
    useEffect(() => console.log("executed after the component is rendered..."));
}
```

### `useEffect()` Clean Up Function

Return a function from the function passed to the `useEffect()` function and it will be called after the component has unmounted.

```jsx
export function FunctionalComponent() {
    useEffect(() => () => console.log("executed after the component was unmounted..."));
}
```
