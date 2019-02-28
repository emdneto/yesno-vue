<template>
  <section class="section">
    <div class="container">
      <div class="columns is-centered">
        <div class="column is-half">
          <h3 class="has-text-danger has-text-weight-light is-size-4">Make a Yes/No question</h3>
        </div>
      </div>

      <div class="columns is-centered">
        <div class="column is-half">
          <b-field>
            <div v-if="loading">
              <b-input v-model="question" loading></b-input>
            </div>
            <div v-else>
              <b-input v-model="question"></b-input>
            </div>
          </b-field>
          <b-loading :is-full-page="false" :active.sync="loading" :can-cancel="true"></b-loading>
          <div v-if="answer === 'Yes!'"><p class="has-text-success">{{ answer }}</p></div>
          <div v-else-if="answer === 'No!'"><p class="has-text-danger">{{ answer }}</p></div>
          <div v-else-if="answer === 'Maybe!'"><p class="has-text-danger">{{ answer }}</p></div>
          <div v-else><p class="has-text-primary">{{ answer }}</p></div>
          
        </div>
      </div>
    </div>
  </section>
</template>

<script>
// Example from: https://br.vuejs.org/v2/guide/computed.html
import _ from "lodash";
import axios from "axios";

export default {
  name: "Main",
  data: function() {
    return {
      question: "",
      answer: "",
      nosymbol: false,
      loading: false
    };
  },
  watch: {
    question: function(newQuestion, oldQuestion) {
      this.answer = "Waiting to finish typing...";
      this.debouncedGetAnswer();
    }
  },
  created: function() {
    this.debouncedGetAnswer = _.debounce(this.getAnswer, 500);
  },
  methods: {
    getAnswer() {
      if (this.question === "") {
        this.answer = "";
        return;
      }
      if (this.question.indexOf("?") === -1) {
        this.nosymbol = true;
        this.answer = "Remember to use the character '?'";
        return;
      }
      this.loading = true;
      this.answer = "Loading...";
      let app = this;
      axios
        .get("https://yesno.wtf/api")
        .then(function(response) {
          app.answer =
            response.data.answer === "yes"
              ? "Yes!"
              : response.data.answer === "no"
              ? "No!"
              : "Maybe!";
          app.loading = false;
        })
        .catch(function(error) {
          app.answer = "Error to call yesno API " + error;
        });
    }
  }
};
</script>


<style>
</style>
