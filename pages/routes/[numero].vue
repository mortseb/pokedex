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
    <div class="pokemons-container">
      <div v-for="pokemon in routeDetails.pokemons" :key="pokemon.slug" class="pokemon-card h-44 w-44 p-4">
        <h2 class="text-white">{{ pokemon.nom }}</h2>
        <img :src="pokemon.artwork.url" :alt="pokemon.nom" />
      </div>
    </div>
  </div>
  <div v-else>
    Chargement des données de la route...
  </div>
</template>

