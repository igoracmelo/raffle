<template>
  <div class="cursor"></div>
</template>

<script>
export default {
  props: {
    id: String,
    x: Number,
    y: Number,
  },

  watch: {
    x(val) {
      this.$el.style.left = `${val}px`;
    },

    y(val) {
      this.$el.style.top = `${val}px`;
    },
  },
};
</script>

<style>
.cursor {
  width: 10px;
  height: 10px;
  background-color: red;
  position: absolute;
  top: 100px;
  left: 100px;
}
</style>