# austlii-datalex

> Project prototype

## Information

It's a Vue.js project based on [webpack template](http://vuejs-templates.github.io/webpack/).
For now it just simulates the request to the server and receives a random sample response

### Response format:
``` js
{
  "text": "A response with both material and conclusion", // consultant's text response
  "conclusion": { // [optional] conclusion object
    "title": "Conclusion #3 reached"
  },
  "material": { // [optional] material object
    "type": "audio",
    "title": "water sounds"
  }
}
```

To add a new response:
1. Create a new json file in `/static/responses`
2. Update the array of availabe responses at component's data `/src/App.vue` –> `data.responses`

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```

For a detailed explanation on how things work, check out the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).
