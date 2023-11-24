<template>
  <div v-if="show" class="modal__wrap" @click="hideModal">
    <div @click.stop class="modal__content">
      <slot></slot>
    </div>
  </div>
</template>

<script>
export default {
  name: "custom-modal",
  props: {
    show: {
      type: Boolean,
      default: false,
    },
  },
  methods: {
    hideModal() {
      this.$emit("update:show", false);
    },
  },
};
</script>

<style scoped>
.modal__wrap {
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  position: absolute;
  display: flex;
  background-color: rgba(0, 0, 0, 0.5);
}
.modal__content {
  margin: auto;
  padding: 20px;
  min-height: 50;
  min-width: 300;
  border-radius: 16px;
  background-color: white;
}
</style>
