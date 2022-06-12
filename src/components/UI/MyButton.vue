<template>
  <button>
      <slot></slot>
  </button>
</template>

<script>
export default {
    name: 'my-button'
};
</script>

<style scoped>
button {
  padding: 10px 15px;
  font-size: 1.5rem;
  align-self: flex-end;
  color: blue;
  background: none;
  border: 1px solid blue;
  cursor: pointer;
  margin-right: 10px;
}
button:last-child {
  margin-right: 0;
}
</style>