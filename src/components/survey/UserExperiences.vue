<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button v-on:click="loadExperiences">
          Load Submitted Experiences</base-button
        >
      </div>

      <p v-if="isLoading">Loading...</p>
      <p v-else-if="!isLoading && error">{{ error }}</p>
      <p v-else-if="!isLoading && (!results || results.length === 0)">
        No stored experiences found. Start adding som experiences first.
      </p>
      <ul v-else>
        <SurveyResult
          v-for="result in results"
          :key="result.id"
          :name="result.name"
          :rating="result.rating"
        />
      </ul>
    </base-card>
  </section>
</template>

<script>
import SurveyResult from './SurveyResult.vue';

export default {
  //REM0VED PROP AS HANDLED AS A DATA OBJECT
  // props: ['results'],
  components: {
    SurveyResult
  },
  data: () => {
    return {
      results: [],
      isLoading: false,
      error: null
    };
  },
  methods: {
    loadExperiences() {
      this.isLoading = true;
      this.error = null;
      fetch(
        'https://vue-http-demo-d8f01-default-rtdb.firebaseio.com/surveys.json',
        {
          method: 'GET'
        }
      )
        .then(response => {
          if (response.ok) {
            return response.json();
          }
        })
        .then(data => {
          this.isLoading = false;
          // console.log(data);
          const results = [];
          for (const id in data) {
            results.push({
              id: id,
              name: data[id].name,
              rating: data[id].rating  
            });
          }

          this.results = results;
        })
        .catch(() => {
          // console.log(error);
          this.isLoading = false;
          this.error = 'Failed to fetch data. Please try again later.';
        });
    }
  },
  mounted() {
    this.loadExperiences();
  }
};
</script>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>
