# Bedrock Development Vue

# Installation

### add to your app.js

```php
import {
  RoutesOverview
} from 'bedrock-development-vue';
Vue.component('RoutesOverview', RoutesOverview)
```

### add translations

to translation loader index

```php
import { translations as bedrockDevelopmentTranslations } from 'bedrock-development-vue';
...
'bedrock-development': { ...bedrockDevelopmentTranslations.nl },
```

`

### Use Locally

```
npm i -D ../bedrock-development-vue
```

add to your package.json

```php
"bedrock-development-vue": "file:../../packages/vue/bedrock-development-vue"
```

Run in your main app

```php
npm install
npm run prod
```

### Use Npm version

```
npm i -D yormy-bedrock-development-vue
```

# Using components

```
import { HelloComponent } from "bedrock-vu--component";

components: {
    HelloComponent
}
  
```

# Building & Publishing

## step 1 build the package

```
npm install
npm run build
```

## step 2: Publish

### setup for npm

``` 
npm adduser
```

### publish

```
npm publish
```

# Typescript

``` 
<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import { SampleComponent } from "vue-component-library";

@Component({
  components: {
    SampleComponent
  }
})
export default class App extends Vue {}
</script>
```
