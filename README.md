# art-thesaurus

> A Vue.js project

We are using the GLoVE vector representation of words to convert our database of words into vectors. Then we find how “similar” two words are by finding the distance of the vectors, in our case we have used the cosine similarity. This calculates the angle between the two vectors. We can think of two words being very similar if their vector representation have similar orientation which translates to a small angle. In the limit, when the vectors have exactly the same orientation, the angle will be zero and cosine of zero is 1 (in our tool we have scaled this up by a factor of 100). When the angle is 90 degrees, the vectors are said to be “orthogonal” and most “dissimilar” or uncorrelated and cosine similarity is 0. Finally when the angle is 180 degrees the vectors have exactly the opposite orientation and the cosine similarity is -1.

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
