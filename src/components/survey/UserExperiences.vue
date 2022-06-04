<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button @click="loadResults">Load Submitted Experiences</base-button>
      </div>
      <ul>
        <survey-result
            v-for='result in results'
            :key='result.id'
            :result='result'
        ></survey-result>
      </ul>
    </base-card>
  </section>
</template>

<script>
import SurveyResult from './SurveyResult.vue'
import axios from "axios"

const FIREBASE_DB = 'https://vue-http-demo-d316d-default-rtdb.europe-west1.firebasedatabase.app/';

export default {
  components: {SurveyResult},
  data() {
    return {
      results: []
    }
  },
  methods: {
    loadResults() {
      axios.get(FIREBASE_DB + 'surveys.json')
          .then(response => {
            this.results = []
            for (let id in response.data)
              this.results.push({id, ...response.data[id]})
          })
    },
  },
  mounted() {
    this.loadResults()
  }
}
</script>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>