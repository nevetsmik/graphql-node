<template>
  <div>
    <Modal v-if="isNewPetModalOpen">
      <NewPet @close-modal="closeModal" @submit="submit" />
    </Modal>
    <div v-else class="page pets-page">
      <section>
        <div class="row betwee-xs middle-xs">
          <div class="col-xs-10">
            <h1>Pets</h1>
          </div>

          <div class="col-xs-2">
            <button @click="isNewPetModalOpen = true">new pet</button>
          </div>
        </div>
      </section>
      <section>
        <div class="row">
          PetsList
        </div>
      </section>
    </div>
  </div>
</template>

<script>
import Modal from "@/components/Modal";
import NewPet from "@/components/NewPet";
import { ref } from "vue";

export default {
  components: {
    Modal,
    NewPet
  },
  setup() {
    const isNewPetModalOpen = ref(false);
    const submit = ({ type, name }) => {
      console.log("type", type);
      console.log("name", name);
    };
    const closeModal = () => {
      isNewPetModalOpen.value = false;
    };
    return { isNewPetModalOpen, submit, closeModal };
  }
};
</script>

<style></style>
