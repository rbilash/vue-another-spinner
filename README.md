# vue-another-spinner

Another Vue.js spinner. 
Configurable. Can act as inside specific component container and also as a global window overlay.
Can be invoked via bound variable, by reference and by emitting event.


*Note: This spinner was made for the needs of specific application and it was not tested under many conditions. In case if you use it and found a bug please open the issue.



### Installation
```
npm install --save vue-another-spinner
```


### Usage example


```javascript
// javascript file

import Spinner from 'vue-another-spinner';

const app = new Vue({
    store,
    router,
    ...
    components: {
        Spinner,
        ...
    },
    data() {
        return {
             loading: false;
        }
    }
}).$mount('#app')

```

```html

<div id="app">
    <spinner global :size="56" label="Loading..." :loading="loading"></spinner>
</div>

```


### Spinner options

+ **global** : Set this to show spiner above all content in browser with fixed positioning, no matter if it called from root component or any child. Default: *false*.
+ **size** : width/height of the spinner, in pixels. Default: *32*.
+ **line** : width of the spinner's line, in pixels. Default: *3*.
+ **shadow** : Color of the grayed area of the circle. Default: *#EEEEEE*.
+ **color** : Color of the spinning area of the circle. Default: *#337ab7*.
+ **speed** : Rotating speed, in seconds. Default: *0.8*.
+ **label** : Text to display below the spinner. Default: *no text*.
+ **label-size** : Size of the text displayed below the spinner, in pixels. Default: *13*.
+ **label-color** : Color of the text displayed below the spinner. Default: *#555555*.
+ **background-color** : Color of the overlay behind the spinner. Default: *rgba(255,255,255,0.9)*.
+ **loading** : On/Off the spinner. Bind a boolean value to this prop to integrate with your component. Default: *false*.
+ **delay** : Minimum delay before showing and hiding spinner to avoid flickering, in ms. Default: *500*.


### Triggering spinner
+ bind value
```html
<spinner :loading="loading"></spinner>
```
```javascript

new Vue({
    ...
    data() {
        return {
             loading: false;
        }
    }
    ...
})

```

+ reference
```html
<spinner ref="myspinner"></spinner>
```
```javascript

new Vue({
    ...
    methods: {
        loadUrl() {
           this.$refs.myspinner.start();
	   axios.get(link).then(...)
              .finally(() => { this.$refs.myspinner.finish(); });
           
        }
    }
    ...
})

```
+ event from child
```html
<spinner global></spinner>
<child></child>
```
```javascript

new Vue({
    name: 'child',
    ...
    methods: {
        loadUrl() {
           this.$emit('spinner-start');
	   axios.get(link).then(...)
              .finally(() => { this.$emit('spinner-finish'); }); 
        }
    }
    ...
})

```
