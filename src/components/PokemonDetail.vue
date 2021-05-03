<template>
  <loader v-if="isLoading" />
  <button class="btn-back" @click="back">Voltar</button>
  <div class="detail-container" v-if="info">
    <div class="div-row">
      <img :src="image" alt="Imagem Pokemon" />
      <h1>{{ info.name }}</h1>
    </div>
    <div class="div-row">
      <h4>Experiência: </h4>
      {{ info.base_experience }}
    </div>
    <div class="div-row">
      <h4>Peso: </h4>
      {{ info.weight }}
    </div>
    <div class="div-row">
      <h4>Altura: </h4>
      {{ info.height }}
    </div>
    <div class="div-images">
    <div  v-for="(value, key, index) in info.sprites" :key="index">
      <img v-if="value && typeof value === 'string'" :src="value" alt="">
    </div>
    </div>
  </div>
</template>

<script>
import { onMounted, ref, computed } from "vue";
import { useRoute, useRouter } from "vue-router";
import { getPokemon, getPokemonImageUrl } from "../service/pokemon-service.js";
import Loader from "./Loader";

export default {
  components: {
    Loader,
  },

  setup() {
    //const router = useRouter();
    const router = useRouter();
    const route = useRoute();
    const info = ref(null);
    const isLoading = ref(false);

    onMounted(() => {
      isLoading.value = true;
      getPokemon(route.params.id)
        .then((resp) => {
          info.value = resp;
          console.log(resp);
        })
        .finally(() => {
          isLoading.value = false;
        });
    });

    const back = () => {
      router.replace("/");
    };

    const image = computed(() => getPokemonImageUrl(info.value.id));

    return { isLoading, info, image, back };
  },
};
</script>


<style scoped>
  .div-row {
    display: flex;
    flex-direction: row;
    align-items: center;
    text-transform: capitalize;
  }

  .btn-back {
    
    border: none;
    background-color: rgb(216, 216, 216) ;
    text-align: center;
    border-radius: 2px;


  }

  .div-images {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    justify-content: center;
  }

</style>
