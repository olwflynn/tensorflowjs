<template lang="html">
  <div>
    <svg width="400" height="180">
      <rect x="50" y="20" width="150" height="150"
      style="fill:blue;"/>
    </svg>
    <label>Find the 5 closest words</label>
    <input type="text" placeholder="e.g Surrealism" v-model="myInput">
    <button>
      Close X
    </button>

    <p v-for='result in results' >{{ myAnswer }} {{ result }}</p>
    <svg width="800" height="300">
      <circle v-bind:cy="y" :cx="x" :r="r" :fill="fill" v-on:click="Transition()" id="myCircle"/>
    </svg>
    <input v-model="inputWord" placeholder="This is a computed try">
    <p v-for="word in lookUp">{{ word }} </p>
    <input v-model="methodWord" placeholder="This is a method try">
    <button v-on:click="methodTry">Find it!</button>
    <p v-for="word in methodUpData">{{ word }} </p>
  </div>
</template>

<script>

export default {
  name: 'Thesaurus',
  props: ['results'],
  data () {
    return {
      myInput: '',
      myAnswer: 'Hello',
      inputWord: '',
      methodWord: '',
      // circle: svg.append('circle').attr('fill', 'purple').attr('cx',200).attr('cy',200).attr('r',50),
      y: 200,
      x: 200,
      r: 30,
      fill: 'purple',
      methodUpData: ['hello', 'goodbye'],
      tensor: ''
    }
  },
  watch: {
    myInput: function () {
      this.myAnswer = this.myInput
      // this.getAnswer()
    }
  },
  methods: {
    Transition: function () {
      this.x = this.x + 100
      this.y = this.y + 50
      this.r = this.r + 100
      var element = document.getElementById('myCircle')
      console.log(element.cx)

      // $('#myCircle').attr('fill', 'blue');
    },
    methodTry: function () {
      this.methodUpData = ['bonsoir']
    }
  },
  computed: {
    lookUp: function () {
      if (this.inputWord.length > 0) {
        const firstResults = ['it works', 'promise']
        console.log(firstResults)
        return firstResults
      } else {
        return ['hello']
      }
    }
  },
  // methods: {
  //   getAnswer : _.debounce(
  //     function() {
  //       this.myInput =
  //     }
  //   )
  // }
  created: function () {
    // `this` points to the vm instance
    console.log('a is: ' + this.myAnswer)
  }
}
</script>

<style lang="css">
p {
    color: red;
    text-align: center;
}

button {
  background-color: Tomato;
  padding-top: 30px;
  padding-right: 50px;
  padding-bottom: 30px;
  padding-left: 50px;
  font-size: 36px;
}

input {
  padding-top: 30px;
  padding-right: 50px;
  padding-bottom: 30px;
  padding-left: 50px;
  font-size: 36px;
}
</style>
