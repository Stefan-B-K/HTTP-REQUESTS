<template>
  <li>
    <header>
    <p>
      <span class="highlight">{{ result.name }}</span> rated the learning experience
      <span :class="ratingClass">{{ result.rating }}</span>.
    </p>
    <base-button mode="flat" @click="$emit('delete-result')">Delete</base-button>
    </header>
  </li>
</template>

<script>
export default {
  emits: ['delete-result'],
  props: ['result'],
  computed: {
    ratingClass() {
      return 'highlight rating--' + this.result.rating;
    },
  },
};
</script>

<style scoped>
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

li {
  margin: 1rem 0;
  border: 1px solid #ccc;
  padding: 1rem;
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