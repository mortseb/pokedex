<script setup>
const query = gql`
  query Route($numero: Int!) {
    route(where: { numero: $numero }) {
      nom
      numero
      pokemons {
        nom
        slug
        description
        artwork {
          url
        }
      }
    }
  }
`;

const routeDetails = ref();
const route = useRoute();
const { data } = await useAsyncQuery(query, {
  numero: parseInt(route.params.numero, 10)
});

watch(() => data.value, (newValue) => {
  if (newValue && newValue.route) {
    routeDetails.value = { ...newValue.route };
    console.log("Route Details:", routeDetails.value);
  }
}, { immediate: true });
</script>


<template>
  <div v-if="routeDetails && routeDetails.pokemons">
    <h1> {{ routeDetails.nom }}</h1>
    <div class="pokemons-container flex flex-col md:flex-row">
      <div v-for="pokemon in routeDetails.pokemons" :key="pokemon.slug"
        class="pokemon-card bg-black m-4 h-fit-content w-fit-content p-4 rounded">
        <NuxtLink :to="`/pokemon/${pokemon.slug}`" class="block cursor-pointer">
          <div
            class="flex flex-col items-center overflow-hidden rounded-lg shadow-md bg-white hover:bg-gray-100  transition duration-300">
            <NuxtImg :src="pokemon.artwork.url" :alt="pokemon.nom"
              class="flex item-center object-cover transition duration-300" />
            <div class="text-center ">
              <h2 class="text-xl md:text-xl lg:text-xl text-center font-semibold">{{ pokemon.nom }}</h2>
            </div>

          </div>
        </NuxtLink>
      </div>
    </div>
  </div>
  <div v-else>
    Chargement des données de la route...
  </div>
</template>

