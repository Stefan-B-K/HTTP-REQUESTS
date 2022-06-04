<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button @click="loadResults">Load Submitted Experiences</base-button>
      </div>
      <p v-if="isLoading">Loading...</p>
      <p v-if="error">{{ error }}</p>
      <p v-else-if="results.length === 0">No stored survey results!</p>
      <ul v-else>
        <survey-result
            v-for='result in results'
            :key='result.id'
            :result='result'
            @delete-result="deleteResult(result.id)"
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
      results: [],
      isLoading: false,
      error: null
    }
  },
  methods: {
    loadResults() {
      this.isLoading = true
      this.error = null
      axios.get(FIREBASE_DB + 'surveys.json')
          .then(response => {
            console.log(response.data)
            this.isLoading = false
            this.results = []
            for (let id in response.data)
              this.results.push({id, ...response.data[id]})
          })
          .catch(error => {
            this.isLoading = false
            this.error = 'Failed to fetch data from server: ' + error.message
          })
    },
    deleteResult(id) {
      this.error = null
      let updatedResults = {}
      this.results = this.results.filter(result => result.id !== id)
      this.results .forEach(result => {
            const {id, ...rest} = result;
            updatedResults[id] = rest;
          })
      axios.put(FIREBASE_DB + 'surveys.json', updatedResults)
          .catch(error => {
            this.error = 'Failed to delete data from server: ' + error.message
          })
    }
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