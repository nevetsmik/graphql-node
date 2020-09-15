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
        <div v-if="isLoadingPets || isNewPetLoading">
          <Spinner />
        </div>
        <div v-if="isErrorLoadingPets || hasNewPetError">
          Error
        </div>
        <div v-else-if="pets" class="row">
          <div
            v-for="pet in pets"
            :key="pet.id"
            className="col-xs-12 col-md-4 col"
          >
            <div className="box">
              <PetBox :pet="pet" />
            </div>
          </div>
        </div>
      </section>
    </div>
  </div>
</template>

<script>
import Modal from "@/components/Modal";
import NewPet from "@/components/NewPet";
import PetBox from "@/components/PetBox";
import Spinner from "@/components/Spinner";
import { ref } from "vue";
import { useQuery, useResult, useMutation } from "@vue/apollo-composable";
import gql from "graphql-tag";

const ALL_PETS = gql`
  query pets {
    pets {
      id
      img
      name
      type
    }
  }
`;

const CREATE_PET = gql`
  mutation createNewPet($input: NewPetInput!) {
    newPet(input: $input) {
      id
      img
      name
      type
    }
  }
`;

export default {
  components: {
    Modal,
    NewPet,
    PetBox,
    Spinner
  },
  setup() {
    const isNewPetModalOpen = ref(false);
    const submit = ({ type, name }) => {
      createNewPet(
        { input: { type, name } },
        {
          update: (cache, { data: { newPet } }) => {
            let { pets } = cache.readQuery({ query: ALL_PETS });
            pets = [newPet].concat(pets);
            cache.writeQuery({
              query: ALL_PETS,
              data: { pets }
            });
          }
        }
      );
    };
    const closeModal = () => {
      isNewPetModalOpen.value = false;
    };

    const {
      result,
      loading: isLoadingPets,
      error: isErrorLoadingPets
    } = useQuery(ALL_PETS);
    const pets = useResult(result, [], result => result.pets);
    const {
      mutate: createNewPet,
      loading: isNewPetLoading,
      error: hasNewPetError
    } = useMutation(CREATE_PET);

    return {
      isNewPetModalOpen,
      submit,
      closeModal,
      pets,
      isLoadingPets,
      isErrorLoadingPets,
      isNewPetLoading,
      hasNewPetError
    };
  }
};
</script>

<style></style>
