<template>
  <div class="header-pokemon">
  <loader v-if="isLoading"/>
  <img src="../assets/pokemon-png-logo.png" alt="">
  <div class ="input-search" >
    <input type="text" placeholder="Pokemon..." value="" >
    </div>
  </div>
  <div class="div-pokemon" v-for="(pokemon, index) in list" :key="index" @click="selectPokemon(getPokemonId(pokemon.url))">
    <img :src="getPokemonImage(getPokemonId(pokemon.url))" />
    <span>{{ pokemon.name }}</span>

  </div>
</template>

<script>
import {
  getPokemonList,
  getPokemonImageUrl,
} from "../service/pokemon-service.js";
import { onMounted, ref } from "vue";
import { useRouter } from "vue-router";
import Loader from "./Loader"

export default {
  components: {
    Loader,
  },

  setup() {
    //criou uma lista para receber os valores
    const list = ref([]);
    const router = useRouter();
    const isLoading = ref(false);
   



    onMounted(() => {
      isLoading.value = true

      //como é async ele precisa usar o then

      getPokemonList().then((resp) => {
        //passando o list (utilizand o value pq é ref e jogando para o resp.results que é um array dos pokemons)
        list.value = resp.results;
      }).finally(() => {
        isLoading.value = false;
      });
    });

    const getPokemonId = (url) =>
      url.replace("https://pokeapi.co/api/v2/pokemon/", "").replace("/", "");

    const getPokemonImage = (id) => getPokemonImageUrl(id);

    const selectPokemon = (id) => router.push({ name: "pokemon", params: { id }});



    return { list, getPokemonImage, getPokemonId, selectPokemon, isLoading };
  }
};
</script>

<style scoped>

  h2{
    text-align: center;


  }

  .header-pokemon {
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;



    
    
  }

  .header-pokemon img {
    max-width: 10rem;
    margin-bottom: 2rem;

 

  }


    .input-search input{
    border-radius: 0.2rem;
    background-color: rgb(216, 216, 216) ;

  }
  

.div-pokemon {

  border: 1px solid rgb(73, 73, 73) ;
  display: flex;
  align-items: center;
  border-radius: 2px;
  margin: 1.5rem;
  cursor: pointer;
  box-shadow: .5rem .25rem .25rem rgb(44, 44, 44);

}

.div-pokemon img {
  background-color:  rgb(61, 61, 61);;
}


.div-pokemon span {
  margin-left: 1rem;
  text-transform: capitalize;

}





</style>
