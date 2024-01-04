<script setup>


import gsap from 'gsap';
import { ScrollTrigger } from 'gsap/ScrollTrigger';
import { onMounted } from 'vue';


const query = gql`
query Combined {
  pokemonsData: pokemons(first: 150) {
    createdAt
    description
    genre
    id
    nom
    poids
    pointsDeVie
    publishedAt
    slug
    taille
    updatedAt
    couleur {
      hex
    }
    artwork {
        url(
        transformation: {
          image: { resize: { fit: crop, height: 1024, width: 1024 } }
          document: { output: { format: webp } }
        }
      )
    }
    types {
      nom
      image {
        url
      }
    }

  }
  typesData: types(first: 20) {
    nom
    couleur {
      hex
    }
    image {
        url(
        transformation: {
          image: { resize: { fit: crop, height: 1024, width: 1024 } }
          document: { output: { format: webp } }
        }
      )
    }
  }
}
`;

const pokemons = ref();
const types = ref();
const selectedTypes = ref([]);
const selectionMode = ref("et");
const filteredPokemons = ref([]);
const searchQuery = ref('');


const { data } = await useAsyncQuery(query);
pokemons.value = data.value.pokemonsData;
types.value = data.value.typesData;


const toggleTypeSelection = (type) => {
  const index = selectedTypes.value.indexOf(type);
  if (index >= 0) {
    selectedTypes.value.splice(index, 1);
  } else {
    if (selectionMode.value === 'ou' || selectedTypes.value.length < 2) {
      selectedTypes.value.push(type);
    } else {
    }
  }
};


const toggleSelectionMode = () => {
  if (selectionMode.value === 'ou') {
    selectedTypes.value = [];
  }
  selectionMode.value = selectionMode.value === 'ou' ? 'et' : 'ou';
};


onMounted(() => {
  gsap.registerPlugin(ScrollTrigger);

  const textElements = gsap.utils.toArray('.text');

  textElements.forEach(text => {
    gsap.to(text, {
      backgroundSize: '100%',
      ease: 'none',
      scrollTrigger: {
        trigger: text,
        start: 'center 80%',
        end: 'center 20%',
        scrub: true,
      },
    });
  });

});


const scrollToPokedex = (event) => {
  event.preventDefault();

  setTimeout(() => {
    const pokedexContainer = document.getElementById('pokedexContainer');
    if (pokedexContainer) {
      pokedexContainer.scrollIntoView({ behavior: 'smooth' });
    }
  }, 50);
};


const removeType = (typeToRemove) => {
  const index = selectedTypes.value.indexOf(typeToRemove);
  if (index > -1) {
    selectedTypes.value.splice(index, 1);
  }
};

watch([selectedTypes, searchQuery], () => {
  let tempFiltered = pokemons.value;

  if (selectedTypes.value.length > 0) {
    tempFiltered = tempFiltered.filter(pokemon =>
      selectionMode.value === 'ou'
        ? pokemon.types.some(type => selectedTypes.value.includes(type.nom))
        : selectedTypes.value.every(selType =>
          pokemon.types.map(type => type.nom).includes(selType))
    );
  }

  if (searchQuery.value) {
    tempFiltered = tempFiltered.filter(pokemon =>
      pokemon.nom.toLowerCase().includes(searchQuery.value.toLowerCase())
    );
  }

  filteredPokemons.value = tempFiltered;
}, { deep: true, immediate: true });

</script>
<template>
  <div class="fixed top-1/2 right-5 z-50">
    <a href="#" ref="scrollArrow" @click="scrollToPokedex"
      class="text-white text-4xl bg-blue-500 hover:bg-blue-600 p-2 rounded-full shadow-lg">
      ↓
    </a>
  </div>


  <div class="h-[500px] overflow-hidden h-screen w-full flex items-center justify-center ">
    <div class="absolute inset-0 bg-cover bg-center w-full" style="background-image: url('/banner.png');"></div>

    <!-- Dégradé en haut -->
    <div class="absolute top-0 left-0 right-0 h-1/3 bg-gradient-to-b from-[#000000] to-transparent"></div>

    <!-- Dégradé en bas -->
    <div class="absolute bottom-0 left-0 right-0 h-1/3 bg-gradient-to-t from-[#000000] to-transparent"></div>

    <!-- Dégradé à gauche -->
    <div class="absolute top-0 bottom-0 left-0 w-1/3 bg-gradient-to-r from-[#000000] to-transparent"></div>

    <!-- Dégradé à droite -->
    <div class="absolute top-0 bottom-0 right-0 w-1/3 bg-gradient-to-l from-[#000000] to-transparent"></div>

    <div class="absolute inset-0 flex justify-center items-center">
      <div class="bg-white bg-opacity-30 backdrop-filter backdrop-blur-lg p-5 rounded-lg">
        <h1 class="text-5xl md:text-9xl  font-bold text-white">MythiDex</h1>



      </div>

    </div>
  </div>





  <div class="containergsap h-auto my-16 w-full">
    <h1 class="text uppercase">pokédex<span class="uppercase">PLUS DE 50 pokémons</span></h1>
    <h1 class="text">MONDIAL<span class="uppercase">1 POKéMON = 1 PAYS</span></h1>
    <h1 class="text uppercase">CRéé PAR IA.<span class="uppercase">POKéMONS GéNéRéS PAR CHATGPT</span></h1>
    <br> <br> <br> <br>
    <h1 class="text">SAFARI MORTSEB<span>
        <NuxtLink to="/carte/">
          CARTE
        </NuxtLink>

      </span></h1>
    <h1 class="text">REGISTRE<span><a href="#" @click="scrollToPokedex">LISTE DES CREATURES</a></span></h1>

  </div>
  <div class="container mx-auto p-4 bg-white shadow-lg rounded-lg pb-96 " id="pokedexContainer">
    <div class="search-bar">
      <input v-model="searchQuery" type="text" placeholder="Rechercher un Pokémon...">
    </div>

    <div class="selected-types flex items-center p-4">
      <h2 class="text-2xl font-semibold mr-3">Types sélectionnés</h2>
      <div class="flex flex-wrap">
        <div v-for="types in selectedTypes" :key="types"
          class="bg-blue-200 text-blue-800 text-xs font-semibold mr-2 px-2.5 py-0.5 rounded dark:bg-blue-200 dark:text-blue-800 flex items-center">
          <div class="cursor-pointer mr-1" @click="removeType(types)">✕</div>
          {{ types }}
        </div>
      </div>
    </div>



    <div class="flex flex-row md:flex-row justify-center md:w-full">
      <div class="text-sm font-medium mr-2">ET</div>
      <label class="relative inline-flex items-center cursor-pointer mx-2">
        <input @click="toggleSelectionMode" type="checkbox" value="" class="sr-only peer">
        <div
          class="w-11 h-6 bg-gray-200 peer-focus:outline-none peer-focus:ring-4 peer-focus:ring-blue-300 dark:peer-focus:ring-blue-800 rounded-full peer dark:bg-gray-700 peer-checked:after:translate-x-full rtl:peer-checked:after:-translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:start-[2px] after:bg-white after:border-gray-300 after:border after:rounded-full after:h-5 after:w-5 after:transition-all dark:border-gray-600 peer-checked:bg-blue-600">
        </div>
      </label>
      <div class="text-sm font-medium mr-2">OU</div>
    </div>

    <div class="flex flex-col md:flex-col divide-y-4">
      <div class="flex flex-wrap w-full md:w-full h-fit pb-8">
        <div v-for="typedata in types" :key="typedata.nom" class="w-1/2 md:w-1/6 p-2">
          <div class="card bg-white rounded-lg shadow hover:shadow-md transition duration-300">
            <input type="checkbox" :id="'checkbox-' + typedata.nom" :checked="selectedTypes.includes(typedata.nom)"
              @change="() => toggleTypeSelection(typedata.nom)" class="hidden">
            <label :for="'checkbox-' + typedata.nom" class="flex items-center cursor-pointer p-4"
              :class="{ 'bg-green-200 border-green-500': selectedTypes.includes(typedata.nom) }">
              <img :src="typedata.image.url" :alt="typedata.nom" class="w-8 h-8 rounded-full mr-1 object-cover">
              <div class="text-xs font-medium">{{ typedata.nom }}</div>
            </label>
          </div>
        </div>
      </div>


      <div class="w-full md:w-full pt-8">
        <ul class="flex flex-wrap">
          <li v-for="pokemon in filteredPokemons" :key="pokemon.id" class="m-2 w-auto  md:w-1/12 lg:w-1/12	">
            <NuxtLink :to="`/pokemon/${pokemon.slug}`" class="block">
              <div
                class="flex flex-col items-center overflow-hidden rounded-lg shadow-md bg-white hover:bg-gray-100 transition duration-300">
                <NuxtImg :src="pokemon.artwork.url" :alt="pokemon.nom"
                  class="flex w-16 item-center h-16 object-cover transition duration-300" />
                <div class="text-center ">
                  <h2 class="text-xs md:text-xs lg:text-xs text-center font-semibold">{{ pokemon.nom }}</h2>
                </div>

              </div>
            </NuxtLink>
          </li>
        </ul>
      </div>
    </div>

  </div>
</template>


