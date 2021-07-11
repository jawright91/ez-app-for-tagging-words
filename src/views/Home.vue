<template>
  <div class="home">
    <h1>{{ this.activeSentence.sentence }}</h1>
    <form>
      <div v-for="word in this.activeSentence.tokenizedWords" v-bind:key="word.id">
        <p>
          {{ word.title }}:
          <select v-model="word.tag">
            <option>NOT AN ENTITY</option>
            <option>PERSON: People, including fictional.</option>
            <option>NORP: Nationalities or religious or political groups.</option>
            <option>FAC: Buildings, airports, highways, bridges, etc.</option>
            <option>ORG: Companies, agencies, institutions, etc.</option>
            <option>GPE: Countries, cities, states.</option>
            <option>LOC: Non-GPE locations, mountain ranges, bodies of water.</option>
            <option>PRODUCT: Objects, vehicles, foods, etc. (Not services.)</option>
            <option>EVENT: Named hurricanes, battles, wars, sports events, etc.</option>
            <option>WORK_OF_ART: Titles of books, songs, etc.</option>
            <option>LAW: Named documents made into laws.</option>
            <option>LANGUAGE: Any named language.</option>
            <option>DATE: Absolute or relative dates or periods.</option>
            <option>TIME: Times smaller than a day.</option>
            <option>PERCENT: Percentage, including ”%“.</option>
            <option>MONEY: Monetary values, including unit.</option>
            <option>QUANTITY: Measurements, as of weight or distance.</option>
            <option>ORDINAL: “first”, “second”, etc.</option>
            <option>CARDINAL: Numerals that do not fall under another type.</option>
          </select>
        </p>
      </div>
    </form>
    <button v-on:click="previousSentence()">Previous</button>
    <button v-on:click="nextSentence()">Next</button>
    <button v-on:click="save()">Save</button>
    <h2>data:</h2>
    <p v-for="sentence in newHotness" v-bind:key="sentence.id">{{ sentence }}</p>
    <h2>Active Sentence:</h2>
    <p>{{ activeSentence }}</p>
  </div>
</template>

<script>
export default {
  data: function () {
    return {
      jsonReturn: [
        { id: 1, sentence: "This is an example sentence." },
        { id: 2, sentence: "This is a better example sentence, with more words and shit." },
      ],
      newHotness: [],
      activeSentence: {},
      activeSentenceIndex: 0,
    };
  },
  created: function () {
    this.tokenify();
    this.activate();
  },
  methods: {
    tokenify() {
      this.newHotness = this.jsonReturn;
      console.log("this should be new hotness", this.newHotness);
      console.log("this should not be", this.jsonReturn);
      for (let i in this.newHotness) {
        var arrayOfWords = this.newHotness[i].sentence.split(" ");
        var tokenizedWords = [];
        var id = 1;
        for (let word in arrayOfWords) {
          var newTokenizedWord = { id: id, title: arrayOfWords[word], tag: null };
          id += 1;
          tokenizedWords.push(newTokenizedWord);
        }
        this.newHotness[i].tokenizedWords = tokenizedWords;
      }
    },
    activate() {
      this.activeSentence = this.newHotness[0];
    },
    nextSentence() {
      this.$set(this.newHotness, this.activeSentenceIndex, this.activeSentence);
      if (this.activeSentenceIndex <= this.newHotness.length - 2) {
        this.activeSentenceIndex += 1;
        this.activeSentence = this.newHotness[this.activeSentenceIndex];
      }
    },
    previousSentence() {
      this.$set(this.newHotness, this.activeSentenceIndex, this.activeSentence);
      if (this.activeSentenceIndex >= 1) {
        this.activeSentenceIndex -= 1;
        this.activeSentence = this.newHotness[this.activeSentenceIndex];
      }
    },
    save() {
      this.$set(this.newHotness, this.activeSentenceIndex, this.activeSentence);
    },
  },
};
</script>
