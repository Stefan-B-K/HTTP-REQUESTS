<template>
  <button :class="mode">
    <slot></slot>
  </button>
</template>

<script>
export default {
  props:['mode'],
};
</script>

<style scoped>
button {
  font: inherit;
  border: 1px solid #360032;
  background-color: #360032;
  color: white;
  padding: 0.5rem 2rem;
  cursor: pointer;
}

button:hover,
button:active {
  background-color: #5c0556;
  border-color: #5c0556;
}

.flat {
  background-color: transparent;
  color: #3a0061;
  border: none;
  font-size: 14px;
}

.flat:hover,
.flat:active {
  background-color: #edd2ff;
}
</style>