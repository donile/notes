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


