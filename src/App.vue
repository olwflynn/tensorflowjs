<template>
  <div id="app">
    <div class="intro">
      <img src="./assets/artimbarc-logo.png">
      <h1>The ARTimbarc Art Thesaurus</h1>
      <h2>An experiment using using Tensorflow.js and Natural Language Processing to find how similar words are to each other</h2>
      <p>We are calculating the cosine similarity. This ranges from -100 (exactly the opposite) to +100 (exactly the same) with 0 meaning no correlation</p>
    </div>
    <div class="content">
      <div style="text-align: center" class="words">
        <h2 text-align:center>Click on two words to calulate their similarity</h2>
        <span class="html_rect" style="background-color:rgb(0, 157, 148)" v-for="(word,index) in targetList" v-on:click="pickWord(index, word)" :id="index">{{word}}</span>
      </div>
      <p>{{ warningText }}</p>
      <button v-on:click="getSimilarity()" type="submit">How similar are these words?</button>
      <button type="submit" name="reset" v-on:click="resetData()">Reset</button>
      <svg width="160" height="180">
        <circle v-bind:cy="y" :cx="x" :r="r" :fill="fill" v-on:click="increaseRadius()" id="myCircle"/>
        <text x="80" y="100" alignment-baseline="middle" text-anchor="middle" class="result" fill="White">{{ similarityMeasure }}</text>
      </svg>
    </div>
  </div>
</template>

<script>
import Thesaurus from './components/Thesaurus'
import * as tf from '@tensorflow/tfjs'

export default {
  name: 'App',
  components: {
    // HelloWorld,
    Thesaurus
  },
  data () {
    return {
      similarityMeasure: '',
      targetList: ['yellow', 'conjuring', 'comically', 'looking', 'electricity', 'superficially', 'picaresque', 'wooded', 'prize', 'wooden', 'spanish', 'vote', 'open', 'city', 'boulevard', 'draft', 'boyfriend', 'depressed', 'rival', 'future', 'glossy', 'simmering', 'russia', 'prospect', 'dutifully', 'sumptuous', 'prestigious', 'argument', 'turner', 'corollary', 'surprise', 'condition', 'contrast', 'revealing', 'full'],
      targetIndex: [],
      targetWords: [],
      y: 100,
      x: 80,
      r: 30,
      fill: 'Black',
      warningText: '',
      offset: 0

    }
  },
  methods: {

    increaseRadius: function () {
      this.r = 80
    },

    resetData: function () {
      this.similarityMeasure = ''
      this.targetWords = []
      this.warningText = ''
      this.r = 30
      for (var i = 0; i < this.targetList.length; i++) {
        document.getElementById(i).style.opacity = '1'
      }
    },

    pickWord: function (index, word) {
      document.getElementById(index).style.opacity = '0.3'
      this.targetIndex.push(index)
      this.targetWords.push(word)
    },

    getJson: async function () {
      console.log('Getting Data')
      const data = await fetch('/static/art_embeddings_v1.json')
      var jsonResponse = data.json()
      console.log('Got the Data')
      return jsonResponse
    },

    fillEmbeddingsObject: function (data) {
      var tensorObject = {}
      for (var word in data) {
        var array = data[word]
        tensorObject[word] = tf.tensor1d(array)
      }
      return tensorObject
    },

    getTensorObject: async function () {
      var embeddings = await this.getJson()
      console.log('Getting Tensors')
      var tensorObjectFinal = await this.fillEmbeddingsObject(embeddings)
      console.log('Got the Tensors')
      return tensorObjectFinal
    },

    getSimilarity: async function () {
      if (this.targetWords.length === 2) {
        var target = this.targetWords[0]
        var test = this.targetWords[1]
        var tensorObject = await this.getTensorObject()
        console.log('Getting Similarity')
        var targetTensor = tensorObject[target]
        var testTensor = tensorObject[test]
        console.log(targetTensor, 'target')
        console.log(testTensor, 'test')

        var dot = tf.sum(tf.mul(targetTensor, testTensor))
        var denom = tf.mul(tf.norm(targetTensor), tf.norm(testTensor))

        var similartiyPromise = new Promise(function (resolve, reject) {
          var similarity = tf.mul(dot, denom.reciprocal()).data()
          if (typeof similarity === 'object') {
            resolve(similarity)
          } else {
            reject(Error('its broken'))
          }
        })
        similartiyPromise.then((result) => {
          console.log(result)
          this.similarityMeasure = Math.round(result[0].toFixed(2) * 100)
          this.increaseRadius()
        }, { function (err) {
          console.log(err)
        }
        })
      } else {
        this.warningText = 'Please click two words'
      }
    }
  }
}

</script>

<style>
#app {
  align-items: stretch;
  display: flex;
  flex-direction: column;
  font-family: 'Ubuntu', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  justify-content: center;
  margin: 0 auto;
  min-height: 100vh;
}
p {
  font-family: 'Ubuntu';
  font-size: 18px;
  color: Black;
  text-align: center;
  margin:50px;
}
img {
  width:100px;
  height:100px;
}

button[type="submit"] {
  display: block;
  margin: 24px auto 0 auto;
  border-radius: 4px;
  font-size: 16px;
}
input[type="input"] {
  margin: 24px auto 0 auto;
}
.intro {
  text-align: center;
  margin-left: auto;
  margin-right: auto;
}

svg {
  margin-left: auto;
  margin-right: auto;
  display: block;
  font-family: 'Ubuntu';
}
.svg_rect {
  fill: rgb(0, 157, 148);
}
.result {
  font-size: 36px;
}

.html_rect {
  border-radius: 4px;
  font-size: 16px;
  margin: 10px 10px 10px 10px;
  padding: 20px;
  text-align: center;
  display:inline-block;
}
</style>
