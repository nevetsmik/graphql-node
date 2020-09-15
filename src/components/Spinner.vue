<template>
  <ClipLoader size="150px" color="#3454D1" :loading="loading" />
</template>

<script>
import ClipLoader from "vue-spinner/src/ClipLoader.vue";

export default {
  components: {
    ClipLoader
  },
  data() {
    return {
      loading: true
    };
  }
};
</script>

<style></style>
