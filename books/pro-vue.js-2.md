# Pro Vue.js 2

## Chapter 17 - Component Lifecycle

### Vue Component Lifecycle Methods

| Method Name | Description |
|-------------|-------------|
| beforeCreate() | Executed before configuration object in Vue component's `<script>` is processed.  Data properties, computed properties and methods are not available. |
| created() | Executed after the configuration object in Vue component's `<script>` is processed.  The Vue component's data properties, computed properties and methods are available. Use to load data from RESTful APIs. |
| beforeMount() | Executed before the HTML in the Vue Component's `<template>` is inserted into the DOM. |
| mounted() | Executed after the Vue component's `<template>` is inserted into the DOM. |
| watcher methods | Watcher methods are executed after the data property they are watching changes. |
| beforeUpdate() | Executed after a data property changes, but before the DOM is updated. |
| updated() | Executed after DOM is updated to reflect data property changes. |
| beforeDestroy() | Executed before Component is removed from the DOM |
| destroyed() | Executed after Component is removed from the DOM and used to remove watchers, event handlers and child components. |
| errorCaptured() | Handles errors thrown in data property modification, watchers and computed properties but NOT events. |

## Chapter 18 - Loosely Coupled Components

### Dependency Injection

#### Defining Services

Services are defined on the `provide` property of the Vue component's configuration object.

Services can be values, functions or objects.

The `provide` property uses the same pattern as the Vue component's configuration object's `data` property; return a function that returns an object.  Each of the object's properties defines a service.

``` vue
<script>
    export default {
        ...
        provide: function() {
            return {
                service1: service1,
                service2: function service2() {},
                serivce3: {},
                ...
                serviceN: {}
            }
        }
        ...
    }
</script>
```

#### Injecting Services

There are two ways to inject services from an antecedent component.

The easiest way to inject a `service` into a component is by adding the `service` name to the array assigned to the configuration object's `inject` property.

``` vue
<script>
    export default {
        ...
        inject: ['service1', 'service2', ... 'serviceN']
        ...
    }
<script>
```
The second way services may be injected into a component is by assiging an object to the configuration object's `inject` property.  Each property of the object assigned to `inject` is the name of a `service` that can be accessed by the component.  The `from` property is the name of the `service` to be injected.  If the service is not found, the `default` factory method is executed, which returns the `service` to be injected.  In the example below, the `default` factory method returns the `service`, which is a function that returns a `value`. 

``` vue
<script>
    export default {
        ...
        inject: {
            renamedService1: 'service1',
            renamedService2: {
                from: 'service2',
                default: () => () => value
            },
            ...
            renamedServiceN: 'serviceN'
        }
        ...
    }
<script>
```

#### Reactive Services
Use `data` properties to hold values used by `service`.  The `provide` property is processed after the `data` property when a Vue component is created from a configuration object.

``` vue
<script>
    export default {
        ...
        data: function() {
            return {
                reactiveValue: 'reactiveString'
            }
        }
        ...
        provide: function() {
            return {
                service: this.reactiveValue
            }
        }
        ...
    }
<script>
```

#### Create and Subscribe to an Event Bus

Create a `service` and assign it a new instance of a Vue object.  Usually done on the top level Vue component.

``` JavaScript
new Vue({
  provide: function() {
      return {
          eventBus: new Vue()
      }
  },
  render: h => h(App)
}).$mount('#app');
```

Inject the eventBus into a component.
``` vue
<script>
    export default {
        ...
        inject: ['eventBus']
        ...
    }
<script>
```

Subscribe to events emitted by the Event Bus
``` vue
<script>
    export default {
        ...
        methods: {
            eventHandler() {
                ...
            }
        },
        ...
        created() {
            ...
            this.eventBus.$on('eventName', this.eventHandler)
        }
        ...
    }
<script>
```

Emit events on the Event Bus
``` vue
<script>
    export default {
        ...
        methods: {
            emitEvent() {
                const payload = {};
                this.eventBus.$emit('eventName', payload);
            }
        }
        ...
    }
<script>
```