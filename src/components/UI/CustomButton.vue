<template>
  <button class="btn">
    <slot></slot>
  </button>
</template>

<script>
export default {
  name: "custom-button",
};
</script>

<style scoped>
.btn {
  background-color: none;
  border: 1px solid teal;
  color: teal;
  padding: 10px 15px;
}
</style>
