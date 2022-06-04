<template>
  <base-card>
    <h2>How was you learning experience?</h2>

    <form @submit.prevent="submitSurvey">

      <div class="form-control">
        <label for="name">Your Name</label>
        <input type="text" id="name" v-model.trim="form.enteredName" />
      </div>

      <h3>My learning experience was ...</h3>
      <div v-for="(rating, key) in ratings" :key="key" class="form-control">
        <input type="radio" :id="key" :value="key" v-model="form.chosenRating" />
        <label :for="key">{{ rating }}</label>
      </div>

      <p v-if="form.invalidInput">
        One or more input fields are invalid. Please check your provided data.
      </p>

      <div>
        <base-button>Submit</base-button>
      </div>

    </form>

  </base-card>
</template>


<script>
import axios from 'axios'

const FIREBASE_DB = 'https://vue-http-demo-d316d-default-rtdb.europe-west1.firebasedatabase.app/';

export default {
  data () {
    return {
      form: { ...this.formDefaults() },
      ratings: { poor: 'Poor', average: 'Average', great: 'Great' }
    };
  },
  methods: {
    submitSurvey () {
      if (this.form.enteredName === '' || !this.form.chosenRating) {
        this.form.invalidInput = true;
        return;
      }

      axios.post(FIREBASE_DB + 'surveys.json', {
        name: this.form.enteredName,
        rating: this.form.chosenRating
      })

      this.form = { ...this.formDefaults() };
    },
    formDefaults () {
      return {
        enteredName: '',
        chosenRating: null,
        invalidInput: false
      };
    }
  }
};
</script>


<style scoped>
.form-control {
  margin: 0.5rem 0;
}

input[type='text'] {
  display: block;
  width: 20rem;
  margin-top: 0.5rem;
}
</style>