<script setup>

// Imports pour les animations au scroll
import gsap from 'gsap';
import { ScrollTrigger } from 'gsap/ScrollTrigger';


//Query combinée pour récupérer les informations des Pokemon et les informations des types séparément
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
          image: { resize: { fit: crop, height: 512, width: 512 } }
          document: { output: { format: webp } }
        }
      )
    }
    types {
      nom
      image {
      url(
        transformation: {
          image: { resize: { fit: crop, height: 512, width: 512 } }
          document: { output: { format: webp } }
        }
      )      }
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
          image: { resize: { fit: crop, height: 512, width: 512 } }
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


// On récupère les informations de la query dans 2 constantes distinctes
const { data } = await useAsyncQuery(query);
pokemons.value = data.value.pokemonsData;
types.value = data.value.typesData;


// Tableau dynamique qui enregistre ou supprime les types sélectionnés
const toggleTypeSelection = (type) => {
  const index = selectedTypes.value.indexOf(type);

  // Vérifications si on peut ajouter des types
  if (index >= 0) {
    selectedTypes.value.splice(index, 1);
  } else {
    if (selectionMode.value === 'ou' || selectedTypes.value.length < 2) {
      selectedTypes.value.push(type);
    } else {
    }
  }
};

// Gestion de la suppression d'un type lors de la recherche
const removeType = (typeToRemove) => {
  const index = selectedTypes.value.indexOf(typeToRemove);
  if (index > -1) {
    selectedTypes.value.splice(index, 1);
  }
};


// Toggle de sélection de types ( et = le pokemon doit avoir ces types, ou = le pokemon doit avoir au moins 1 de ces types)
const toggleSelectionMode = () => {
  if (selectionMode.value === 'ou') {
    selectedTypes.value = [];
  }
  selectionMode.value = selectionMode.value === 'ou' ? 'et' : 'ou';
};


// Animations GSAP au scroll
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

// Gestion de la navigation automatique jusqu'à la liste
const scrollToPokedex = (event) => {
  event.preventDefault();
  setTimeout(() => {
    const pokedexContainer = document.getElementById('pokedexContainer');
    if (pokedexContainer) {
      pokedexContainer.scrollIntoView({ behavior: 'smooth' });
    }
  }, 50);
};



// Gestion dynamique des résultats de la recherche
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
<style></style>
<template>
  <!-- Bouton de navigation jusqu'à la liste -->
  <div class="fixed top-1/2 right-5 z-50">
    <a href="#" ref="scrollArrow" @click="scrollToPokedex"
      class="text-white text-4xl bg-blue-500 hover:bg-blue-600 p-2 rounded-full shadow-lg">
      ↓
    </a>
  </div>

  <!-- Banniere et titre -->
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

    <!-- Titre -->
    <div class="absolute inset-0 flex justify-center items-center">
      <div class="bg-black/30 backdrop-filter backdrop-blur-lg p-5 rounded-full">
        <h1 class="text-5xl md:text-9xl  font-bold text-white titresite">MythiDex</h1>
      </div>

    </div>
  </div>




  <!-- Section de présentation du site -->
  <div class="containergsap h-auto my-16 w-full">
    <h1 class="text uppercase">pokédex<span class="uppercase">PLUS DE 50 pokémons</span></h1>
    <h1 class="text">MONDIAL<span class="uppercase">1 POKéMON = 1 PAYS</span></h1>
    <h1 class="text uppercase">CRéé PAR IA.<span class="uppercase">POKéMONS PAR CHATGPT</span></h1> <br> <br> <br> <br>

    <!-- Liens vers la carte / la liste -->
    <h1 class="text">SAFARI MORTSEB
      <span>
        <NuxtLink to="/carte/">
          CARTE
        </NuxtLink>
      </span>
    </h1>
    <h1 class="text">REGISTRE
      <span>
        <a href="#" @click="scrollToPokedex">LISTE DES CREATURES</a>
      </span>
    </h1>
  </div>


  <!-- Zone de recherche -->
  <div class="container mx-auto p-4 shadow-lg rounded-lg pb-96 bg-cover bg-center w-full md:h-[1232px]"
    style="background-image: url('/search-bg.png');" id="pokedexContainer">
    <!-- Fond blanc - Div global -->
    <div class="bg-white/30 backdrop-blur p-4 md:h-[1200px] shadow-lg rounded-lg">
      <!-- Fond noir - Div des éléments de recherche -->
      <div class="bg-black/80 backdrop-blur p-4 pb-0 rounded-3xl h-fit mb-8 n">
        <div class="selected-types flex items-center justify-center p-4">
          <div class="w-2/3 max-w-md text-center">
            <!-- Barre de recherche -->
            <div class="search-bar">
              <input v-model="searchQuery" type="text" placeholder="Rechercher un Pokémon..."
                class="rounded-3xl pl-2 w-full">
            </div>


          </div>
        </div>

        <!-- Toggle - recherche par type ET/OU -->
        <div class="flex flex-row md:flex-row justify-center md:w-full mt-8 mb-4 ">
          <div class="flex flex-row md:flex-row justify-center bg-white w-fit p-4 rounded-3xl">
            <div class="text-sm font-medium mr-2">ET</div>
            <label class="relative inline-flex items-center cursor-pointer mx-2">
              <input @click="toggleSelectionMode" type="checkbox" value="" class="sr-only peer">
              <div
                class="w-11 h-6 bg-gray-200 peer-focus:outline-none peer-focus:ring-4 peer-focus:ring-blue-300 dark:peer-focus:ring-blue-800 rounded-full peer dark:bg-gray-700 peer-checked:after:translate-x-full rtl:peer-checked:after:-translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:start-[2px] after:bg-white after:border-gray-300 after:border after:rounded-full after:h-5 after:w-5 after:transition-all dark:border-gray-600 peer-checked:bg-blue-600">
              </div>
            </label>
            <div class="text-sm font-medium mr-2">OU</div>
          </div>
        </div>

        <!-- Liste des types sélectionnables -->
        <div class="flex flex-col md:flex-col divide-y-4 ">
          <div class="flex flex-wrap w-full md:w-full h-fit pb-8 justify-center">
            <div v-for="typedata in types" :key="typedata.nom" class=" w-36  p-2">
              <div class="card bg-white rounded-lg shadow hover:shadow-md transition duration-300">
                <input type="checkbox" :id="'checkbox-' + typedata.nom" :checked="selectedTypes.includes(typedata.nom)"
                  @change="() => toggleTypeSelection(typedata.nom)" class="hidden">
                <label :for="'checkbox-' + typedata.nom" class="flex items-center cursor-pointer p-4 "
                  :class="{ ' rounded-lg shadow hover:shadow-md transition duration-300 bg-green-300 border-green-500': selectedTypes.includes(typedata.nom) }">
                  <img :src="typedata.image.url" :alt="typedata.nom" class="w-8 h-8 rounded-full mr-3 object-cover">
                  <div class="text-sm ld:text-xl font-medium">{{ typedata.nom }}</div>

                </label>
              </div>
            </div>
          </div>
        </div>
        <!-- Liste des types sélectionnés -->
        <div class="flex flex-wrap justify-center mt-2">
          <div v-for="types in selectedTypes" :key="types"
            class="bg-blue-200 text-blue-800 text-xs font-semibold mr-2 px-2.5 py-0.5 rounded flex items-center mb-2">
            <div class="cursor-pointer mr-1" @click="removeType(types)">✕</div>
            {{ types }}
          </div>
        </div>
      </div>

      <!-- Résultats de la recherche - Liste pleine si pas de recherche -->
      <div class="w-full md:w-full pt-8 max-h-[500px] overflow-auto">
        <ul class="flex flex-wrap justify-center">
          <li v-for="pokemon in filteredPokemons" :key="pokemon.id" class="m-2 w-32  md:w-1/6">
            <NuxtLink :to="`/pokemon/${pokemon.slug}`" class="block">
              <div
                class="flex flex-col items-center rounded-lg shadow-md bg-white/50 p-2 hover:bg-gray-100 transition duration-300">
                <NuxtImg :src="pokemon.artwork.url" :alt="pokemon.nom"
                  class="flex w-auto item-center h-auto object-cover transition duration-300" />
                <div class="text-center ">
                  <h2 class="text-xs md:text-xl lg:text-xl text-center font-semibold">{{ pokemon.nom }}</h2>
                </div>

              </div>
            </NuxtLink>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>


