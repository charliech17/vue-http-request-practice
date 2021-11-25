<template>
  <li>
    <p>
      <span class="highlight">{{ name }}</span> rated the learning experience
      <span :class="ratingClass">{{ rating }}</span>.
      <base-button @click="$emit('deleteItem')">delete</base-button>
    </p>
  </li>
</template>

<script>
export default {
  props: ['name', 'rating'],
  emits:['deleteItem']
  ,computed: {
    ratingClass() {
      return 'highlight rating--' + this.rating;
    },
  },
};
</script>

<style scoped>
li {
  margin: 1rem 0;
  border: 1px solid #ccc;
  padding: 1rem;
}

li p{
  position: relative;
  box-sizing: border-box;
  display: block;
}

li p button{
  position: absolute;
  right: 0;
  transform: translateY(-0.5rem);

}

h3,
p {
  font-size: 1rem;
  margin: 0.5rem 0;
}

.highlight {
  font-weight: bold;
}

.rating--poor {
  color: #b80056;
}

.rating--average {
  color: #330075;
}

.rating--great {
  color: #008327;
}
</style>