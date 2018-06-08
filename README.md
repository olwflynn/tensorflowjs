# art-thesaurus

> A Vue.js project

We took as our corpus of words (i.e. the possible outputs of the thesaurus) a relatively small number of art reviews found at various reputable sources. Then we gave the model a “target” word; this is the word we want see context for. Each word can be mapped to a vector by the GLoVE algorithm and then we can use some maths to find the 5 most “similar” or “closest” words in the corpus to the target word.

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
