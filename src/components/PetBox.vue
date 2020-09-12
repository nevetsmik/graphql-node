<template>
  <div class="pet">
    <figure>
      <img :src="`${img}?pet=${id}`" alt="" />
    </figure>
    <div class="pet-name">{{ name }}</div>
    <div class="pet-type">{{ type }}</div>
  </div>
</template>

<script>
import { toRefs } from "vue";
export default {
  props: {
    pet: {
      type: Object,
      default() {
        return {
          id: 123,
          name: "John Doe",
          type: "Android",
          img: "url"
        };
      }
    }
  },
  setup(props) {
    const { id, name, type, img } = toRefs(props);
    return { id, name, type, img };
  }
};
</script>

<style></style>
