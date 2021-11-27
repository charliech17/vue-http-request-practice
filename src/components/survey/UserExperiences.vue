<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button @click="loadExperiences" ref="loadButton"
          >Load Submitted Experiences</base-button
        >
      </div>
      <p v-if="isLoading">Loading...</p>
      <p v-else-if="!isLoading && error">
        {{ error }}
      </p>
      <p v-else-if="!isLoading && (!finalResults || finalResults.length === 0)">
        No stored experiences found. Start adding some survey results first
      </p>
      <ul v-else-if="!isLoading && finalResults.length > 0">
        <survey-result
          v-for="result in finalResults"
          :key="result.id"
          :name="result.name"
          :rating="result.rating"
          :editIndex="thePressIdx"
          @deleteItem="deleteExperience(result.id)"
          @pressUpdate="(name,rating)=>updateEdtion(name,rating,result.id)"
        ></survey-result>
      </ul>
    </base-card>
  </section>
</template>

<script>
import BaseCard from '../UI/BaseCard.vue';
import SurveyResult from './SurveyResult.vue';

export default {
  props: ['results', 'pressLoading'],
  components: {
    SurveyResult,
    BaseCard,
  },
  data() {
    return {
      finalResults: [],
      isLoading: false,
      error: null,
      press: false,
      thePressIdx: -1,
    };
  },
  watch: {
    pressLoading: function () {
      if (this.pressLoading === true) {
        this.loadExperiences();
        this.$emit('pressFinish');
        // this.pressLoading = false;
      }
    },
  },
  methods: {
    loadExperiences() {
      this.isLoading = true;
      this.error = null;
      fetch(
        'https://vue-http-demo-e1e10-default-rtdb.firebaseio.com/surveys.json'
      )
        .then((response) => {
          if (response.ok) {
            return response.json();
          }
        })
        .then((data) => {
          this.isLoading = false;
          const results = [];
          for (const id in data) {
            results.push({
              id: id,
              name: data[id].name,
              rating: data[id].rating,
            });
          }
          this.finalResults = results;
        })
        .catch(() => {
          this.isLoading = false;
          this.error = 'Failed to fetch data - please try again later.';
        });
    },
    deleteExperience(id) {
      fetch(
        `https://vue-http-demo-e1e10-default-rtdb.firebaseio.com/surveys/${id}.json`,
        {
          method: 'Delete',
          headers: {
            'Content-Type': 'application/json',
          },
        }
      )
        .then((response) => {
          if (response.ok) {
            this.loadExperiences();
          } else {
            throw new Error('could not save data');
          }
        })
        .catch((error) => {
          this.error = error.message;
        });
    },
    tellIndexPress(idx){
      this.thePressIdx = idx; 
    },
    updateEdtion(name,rating,id){
      if(name===''||rating===''){
        return;
      }
      this.dataInvalid = false;
      fetch(
        `https://vue-http-demo-e1e10-default-rtdb.firebaseio.com/surveys/${id}.json`,
        {
          method: 'PUT',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify({
            name: name,
            rating: rating,
          }),
        }
      )
        .then((response) => {
          if (response.ok) {
            this.loadExperiences();
          } else {
            throw new Error('could not save data');
          }
        })
        .catch((error) => {
          this.error = error.message;
        });
    }

  },
  mounted() {
    this.loadExperiences();
  },
};
</script>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>