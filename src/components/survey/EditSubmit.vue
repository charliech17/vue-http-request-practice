<template>
  <div class="edit_submit_contents">
    <label for="newName">New Name</label>
    <input
      type="text"
      name="newName"
      id="newName"
      v-model.trim="newEnterName"
    />
    <h3>Edit learning experience was ...</h3>
    <div class="edit_content">
      <input
        type="radio"
        id="rating_poor"
        value="poor"
        name="rating"
        v-model="newChosenRating"
      />
      <label for="rating_poor" class="rating">Poor</label>
      <input
        type="radio"
        id="rating_average"
        value="average"
        name="rating"
        v-model="newChosenRating"
      />
      <label for="rating_average" class="rating">Average</label>
      <input
        type="radio"
        id="rating_great"
        value="great"
        name="rating"
        v-model="newChosenRating"
      />
      <label for="rating_great" class="rating">Great</label>
      <base-button class="change_submit" @click="clickSubmitChange"
        >Change submit</base-button
      >
      <p v-if="inputInvalid" class="empty_alert">
        Some inputs must be empty, please check again
      </p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newChosenRating: null,
      newEnterName: '',
      inputInvalid: false,
    };
  },
  emits: ['updateSubmit'],
  methods: {
    clickSubmitChange() {
      if (this.newEnterName.trim() === '' || this.newChosenRating.trim() === '') {
        this.inputInvalid = true;
        return;
      }
      this.inputInvalid = false;
      this.$emit('updateSubmit', this.newEnterName, this.newChosenRating);
    },
  },
};
</script>

<style scoped>
label[for='newName'] {
  margin-right: 1vh;
}

.rating {
  margin-right: 1vh;
  cursor: pointer;
}

.edit_content {
  position: relative;
}

.edit_content > button {
  position: absolute;
  right: 0;
  transform: translateY(-0.5rem);
}

.change_submit {
  border-radius: 1vh;
  background-color: #d5c6fa;
  color: black;
  font-weight: bold;
  box-shadow: 0 0 0.5vh 0.5vh rgba(0, 0, 0, 0.2);
}

.change_submit:hover,
.change_submit:active {
  color: white;
}

.empty_alert {
  color: red;
}
</style>
