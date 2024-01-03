<script setup>
const query = gql`
query Pokemon($slug: String!) {
  pokemon(where: { slug: $slug }) {
    id
    nom
    slug
    description
    artwork {
      url(
        transformation: {
          image: { resize: { fit: crop, height: 1024, width: 1024 } }
          document: { output: { format: webp } }
        }
      )
    }
    pointsDeVie
    genre
    poids
    taille
    couleur {
      hex
    }
  }
}

`;

const pokemon = ref();

const route = useRoute();
const { data } = await useAsyncQuery(query, {
  slug: route.params.slug,
});

console.log(data.value);
pokemon.value = data.value.pokemon;
console.log(data.value.pokemon);

</script>

<template>
  <div v-if="pokemon" class="max-w-lg">
    <NuxtImg :src="pokemon.artwork.url" :alt="pokemon.nom" />
    <h2 class="text-3xl text-center text-white">{{ pokemon.nom }}</h2>
    <div class="text-white">Desciption : {{ pokemon.description }}</div>
  </div>

  <div v-else>
    <li>Loading...</li>
  </div>
</template>