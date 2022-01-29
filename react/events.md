# Events

## Handling Events

Assign a function to the event property.

The function may have zero or one parameter; the `SyntheticEvent`.

### Handling Events with In-line Functions

```jsx
<div onClick={ () => console.log("handled event") }>

</div>
```

```jsx
<div onClick={ (event) => console.log(`event.type: ${event.type}`) }>

</div>
```

### Invoking Component Method Without Args

```jsx
import React, { Component } from 'react';

export default class MyComponent extends Component {

    handleEvent = () => {
        console.log("handled event");
    }

    render() {
        return (
            <div onClick={ this.handleEvent }>

            </div>
        )
    }
}
```

### Invoking Component Method with Event

```jsx
import React, { Component } from 'react';

export default class MyComponent extends Component {

    handleEvent = (event) => {
        console.log("handled event");
    }

    render() {
        return (
            <div onClick={ this.handleEvent }>

            </div>
        )
    }
}
```

### Invoking Component Method With Custom Args

```jsx
import React, { Component } from 'react';

export default class MyComponent extends Component {

    handleEvent = (event, arg) => {
        console.log(`event.type: ${event.type}`);
    }

    render() {
        return (
            <div onClick={ (e) => this.handleEvent(e, "arg") }>

            </div>
        )
    }
}
```

## Event Lifecycle

Capture Phase (Top-down; `body` element to target element)
Target Phase (target element)
Bubble Phase (target element to `body` element)

### Handle Event in Capture Phase

Set a capture phase prop such as `onClickCapture` prop to an event handling function.

```jsx
<button onClickCapture={ (e) => console.log(`event.type: ${e.type}`) } >
    Click Me!
</button>
```

### Handle Event in the Bubble Phase

Set a bubble phase prop such as `onClick` to an event handling function.

```jsx
<div onClick={ (e) => console.log(`event.type: ${e.type}`) }>
    <button  type="button">Click Me!</button>
</div>
```

### Determine Capture Phase

Pass a flag parameter to the event handler to identify the function was executed by a capture phase prop.

```jsx
handleClick = (e, isCapturePhase) => {
    if (isCapturePhase) {
        console.log("in caputure phase");
    }
}

render() {
    return (
        <button 
            onClickCapture={ (e) => handleClick(e, true) } 
            type="button">
            Click Me!
        </button>
    )
}
```

### Determine Target Phase

Check `event.currentTarget` is equal to `event.target`.

```jsx
handleClick = (e) => {
    if (e.currentTarget === e.target) {
        console.log("in target phase");
    }
}

render() {
    return (
        <button 
            onClickCapture={ (e) => handleClick(e) } 
            type="button">
            Click Me!
        </button>
    )
}
```

### Determine Bubbles Phase

Check `event.bubbles` is `true` and `event.currentTarget` does not equal `event.target`.

```jsx
handleClick = (e) => {
    if (e.bubbles && e.currentTarget !== e.target) {
        console.log("in bubbles phase");
    }
}

render() {
    return (
        <button 
            onClickCapture={ (e) => handleClick(e) } 
            type="button">
            Click Me!
        </button>
    )
}
```

## Pitfalls

### Do Not Reuse Events

React `SyntheticEvent` objects have all their properties set to null after the even has been handled.

Use the `SyntheticEvent`'s `persist() method to instruct React not to discard the event.

```jsx
handleEvent = (event) => {
    event.persist();
    this.setState(
        { value: this.state.value + 1 },
        () => console.log(`event properties still available ${event.type}`)
    );
}
```

### Do Not Add Event Handlers to React Components

React componets are removed from the DOM, only the content the React component returns is inserted into the DOM.  Since the React component is not in the DOM, any event handlers added to the React component are not present.

```jsx
<MyComponent onClick={ () => console.log("never executed") } />
```