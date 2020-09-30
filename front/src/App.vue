<template>
  <div id="app">
    <div class="container">
      <Tag />
    </div>
  </div>
</template>

<script>
import Tag from "./components/Tag.vue";

export default {
  name: "App",
  components: {
    Tag,
  },
};
</script>

<style>
.container {
  height: 500px;
  width: 500px;
}
</style>
