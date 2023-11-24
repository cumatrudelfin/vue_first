<template>
  <form @submit.prevent class="form">
    <h4>Create item :</h4>
    <custom-input v-model="post.title" placeholder="title" />
    <custom-input v-model="post.body" placeholder="body" />

    <custom-button
      @click="createPost"
      style="align-self: flex-end; margin-top: 15px"
      >BUTTON</custom-button
    >
  </form>
</template>

<script>
export default {
  data() {
    return {
      post: {
        title: "",
        body: "",
      },
    };
  },
  methods: {
    createPost() {
      this.post.id = Date.now();
      this.$emit("create", this.post);
      this.post = {
        title: "",
        body: "",
      };
    },
  },
  watch: {
    post: {
      handler(newValue) {},
      deep: true,
    },
  },
};
</script>

<style scoped>
.form {
  display: flex;
  flex-direction: column;
}
</style>
