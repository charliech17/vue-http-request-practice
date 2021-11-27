<template>
  <li @click="toggleButton">
    <p>
      <span class="highlight">{{ name }}</span> rated the learning experience
      <span :class="ratingClass">{{ rating }}</span>.
      <base-button @click="$emit('deleteItem')">delete</base-button>
      <base-card v-if="isShow">
        <edit-submit @click="stopParentEvent" @updateSubmit="(name,rating)=>$emit('pressUpdate',name,rating)"></edit-submit>
      </base-card>
      
    </p>
  </li>
</template>

<script>
import EditSubmit from  './EditSubmit.vue';
export default {
  components:{
    EditSubmit
  }
  ,props: ['name', 'rating'],
  emits:['deleteItem','pressUpdate']
  ,computed: {
    ratingClass() {
      return 'highlight rating--' + this.rating;
    },
  },
  data(){
    return {
      isShow:false
    };
  },
  methods:{
    toggleButton(){
        this.isShow = !this.isShow;
    },
    stopParentEvent(e){
      e.stopPropagation();
    },
  }
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