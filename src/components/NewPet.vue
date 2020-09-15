<template>
  <div className="new-pet page">
    <h1>New Pet</h1>
    <div className="box">
      <form @submit.stop.prevent="createNewPet">
        <select v-model="selectedAnimal" class="input">
          <option
            v-for="animal in animals"
            :value="animal.value"
            :key="animal.value"
            >{{ animal.label }}</option
          ></select
        >
        <input
          v-model="animalName"
          class="input"
          type="text"
          placeholder="pet name"
          required
        />
        <button
          type="button"
          class="error button"
          @click.prevent="emit('close-modal')"
        >
          cancel
        </button>
        <button type="submit">add pet</button>
      </form>
    </div>
  </div>
</template>

<script>
import { ref, reactive } from "vue";

export default {
  setup(props, { emit }) {
    const animals = reactive([
      { value: "DOG", label: "dog" },
      { value: "CAT", label: "cat" }
    ]);
    const animalName = ref("");
    const selectedAnimal = ref(animals[0].value);

    const createNewPet = () => {
      emit("submit", { type: selectedAnimal.value, name: animalName.value });
      emit("close-modal");
    };
    return { animals, animalName, selectedAnimal, emit, createNewPet };
  }
};
</script>

<style></style>
