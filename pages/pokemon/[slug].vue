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
    pays
    pointsDeVie
    genre
    poids
    taille
    couleur {
      hex
    }
    types {
      nom
      image {
        url
      }
    }
    attaques1 {
      nom
      image {
        url
      }
      types {
        nom
        image {
          url(
            transformation: {
              image: { resize: { fit: crop, height: 512, width: 512 } }
              document: { output: { format: webp } }
        }
      )
    }
      }
      degats
    }
    routes {
      nom
    }


  }
}

`;
const activeTabColor = 'bg-white';
const inactiveTabColor = 'bg-slate-00';
const pokemon = ref();
const sectionVisible = ref("description");

const showSection = (section) => {
  sectionVisible.value = section;
};
const route = useRoute();
const { data } = await useAsyncQuery(query, {
  slug: route.params.slug,
});

console.log(data.value);
pokemon.value = data.value.pokemon;
console.log(data.value.pokemon);

</script>

<template>
  <div v-if="pokemon" class="flex flex-row w-full bg-slate-200 rounded py-6 sm:py-12 mt-8	">
    <div class="flex w-auto w-1/3 p-3">

      <NuxtImg :src="pokemon.artwork.url" :alt="pokemon.nom" class="h-auto w-full rounded" />
    </div>
    <div class="flex flex-col w-2/3">
      <div class=" flex justify-center items-start">
        <button @click="showSection('description')"
          :class="[sectionVisible === 'description' ? activeTabColor : inactiveTabColor, 'text-black group flex items-center justify-center rounded-lg px-1 py-1 md:px-3 md:px-3 mx-2 shadow-[-2px_-2px_10px_rgba(255,255,255,1),3px_3px_10px_rgba(0,0,0,0.2)] active:shadow-[inset_-2px_-2px_5px_rgba(255,255,255,0.7),inset_3px_3px_5px_rgba(0,0,0,0.1)]']">
          <span class="sr-only">Home</span>
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor"
            class="h-4 w-4 md:w-6 md:h-6 text-slate-500 group-active:scale-95"
            style="fill-rule:evenodd;clip-rule:evenodd;stroke-linejoin:round;stroke-miterlimit:2;" version="1.1"
            xml:space="preserve" xmlns:serif="http://www.serif.com/" xmlns:xlink="http://www.w3.org/1999/xlink">
            <path
              d="M1,5.998l0,16.002c-0,1.326 0.527,2.598 1.464,3.536c0.938,0.937 2.21,1.464 3.536,1.464c5.322,0 14.678,-0 20,0c1.326,0 2.598,-0.527 3.536,-1.464c0.937,-0.938 1.464,-2.21 1.464,-3.536c0,-3.486 0,-8.514 0,-12c0,-1.326 -0.527,-2.598 -1.464,-3.536c-0.938,-0.937 -2.21,-1.464 -3.536,-1.464c-0,0 -10.586,0 -10.586,0c0,-0 -3.707,-3.707 -3.707,-3.707c-0.187,-0.188 -0.442,-0.293 -0.707,-0.293l-5.002,0c-2.76,0 -4.998,2.238 -4.998,4.998Zm3.792,11.499l1.755,-3.009c0.537,-0.921 1.524,-1.488 2.591,-1.488c0,0 13.815,0 13.815,0c1.067,0 2.053,0.567 2.591,1.488c0.904,1.55 1.028,1.762 1.788,3.066c0.278,0.477 0.891,0.638 1.368,0.36c0.477,-0.278 0.638,-0.891 0.36,-1.368c-0.761,-1.304 -0.885,-1.516 -1.789,-3.065c-0.896,-1.536 -2.54,-2.481 -4.318,-2.481c-3.354,0 -10.462,0 -13.815,0c-1.778,0 -3.423,0.945 -4.319,2.481c0,-0 -1.755,3.009 -1.755,3.009c-0.278,0.476 -0.117,1.089 0.36,1.367c0.477,0.278 1.089,0.117 1.368,-0.36Z" />
            <g id="Icon" />
          </svg>
        </button>
        <button @click="showSection('attaques')"
          :class="[sectionVisible === 'attaques' ? activeTabColor : inactiveTabColor, 'text-black group flex items-center justify-center rounded-lg px-1 py-1 md:px-3 md:px-3 mx-2 shadow-[-2px_-2px_10px_rgba(255,255,255,1),3px_3px_10px_rgba(0,0,0,0.2)] active:shadow-[inset_-2px_-2px_5px_rgba(255,255,255,0.7),inset_3px_3px_5px_rgba(0,0,0,0.1)]']">
          <span class="sr-only">Account</span>
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor"
            class="h-4 w-4 md:w-6 md:h-6 text-slate-500 group-active:scale-95">
            <path fill-rule="evenodd"
              d="M14.615 1.595a.75.75 0 01.359.852L12.982 9.75h7.268a.75.75 0 01.548 1.262l-10.5 11.25a.75.75 0 01-1.272-.71l1.992-7.302H3.75a.75.75 0 01-.548-1.262l10.5-11.25a.75.75 0 01.913-.143z"
              clip-rule="evenodd" />
          </svg>
        </button>

        <button @click="showSection('localisation')"
          :class="[sectionVisible === 'localisation' ? activeTabColor : inactiveTabColor, 'text-black group flex items-center justify-center rounded-lg px-1 py-1 md:px-3 md:px-3 mx-2 shadow-[-2px_-2px_10px_rgba(255,255,255,1),3px_3px_10px_rgba(0,0,0,0.2)] active:shadow-[inset_-2px_-2px_5px_rgba(255,255,255,0.7),inset_3px_3px_5px_rgba(0,0,0,0.1)]']">
          <span class="sr-only">Explore</span>
          <svg xmlns="http://www.w3.org/2000/svg" fill="currentColor"
            class="h-4 w-4 md:w-6 md:h-6 text-slate-500 group-active:scale-95" xmlns:xlink="http://www.w3.org/1999/xlink"
            viewBox="0 0 511.953 511.953" xml:space="preserve">
            <g transform="translate(-1)">
              <g>
                <g>
                  <path d="M256.995,149.287c-11.776,0-21.333,9.579-21.333,21.333c0,11.755,9.557,21.333,21.333,21.333s21.333-9.579,21.333-21.333
				C278.328,158.865,268.771,149.287,256.995,149.287z" />
                  <path d="M365.518,38.887C325.987,6.311,274.04-6.639,223.011,3.239C154.147,16.615,100.152,72.273,88.718,141.735
				c-6.784,41.003,0.725,81.216,21.696,116.267l8.704,14.528c27.861,46.443,56.64,94.485,79.701,143.893l38.848,83.221
				c3.499,7.509,11.029,12.309,19.328,12.309s15.829-4.8,19.328-12.309l34.965-74.923c23.317-49.984,52.096-98.688,79.957-145.792
				l12.971-22.016c15.339-26.091,23.445-55.936,23.445-86.293C427.662,119.484,405.006,71.463,365.518,38.887z M256.995,234.62
				c-35.285,0-64-28.715-64-64s28.715-64,64-64s64,28.715,64,64S292.28,234.62,256.995,234.62z" />
                </g>
              </g>
            </g>
          </svg>
        </button>

        <button @click="showSection('informations')"
          :class="[sectionVisible === 'informations' ? activeTabColor : inactiveTabColor, 'text-black group flex items-center justify-center rounded-lg px-1 py-1 md:px-3 md:px-3 mx-2 shadow-[-2px_-2px_10px_rgba(255,255,255,1),3px_3px_10px_rgba(0,0,0,0.2)] active:shadow-[inset_-2px_-2px_5px_rgba(255,255,255,0.7),inset_3px_3px_5px_rgba(0,0,0,0.1)]']">
          <span class="sr-only">Notifications</span>
          <svg xmlns="http://www.w3.org/2000/svg" fill="currentColor"
            class="h-4 w-4 md:w-6 md:h-6 text-slate-500 group-active:scale-95" xmlns:xlink="http://www.w3.org/1999/xlink"
            viewBox="0 0 512 512" xml:space="preserve">
            <g>
              <path class="st0" d="M255.992,0.008C114.626,0.008,0,114.626,0,256s114.626,255.992,255.992,255.992
		C397.391,511.992,512,397.375,512,256S397.391,0.008,255.992,0.008z M300.942,373.528c-10.355,11.492-16.29,18.322-27.467,29.007
		c-16.918,16.177-36.128,20.484-51.063,4.516c-21.467-22.959,1.048-92.804,1.597-95.449c4.032-18.564,12.08-55.667,12.08-55.667
		s-17.387,10.644-27.709,14.419c-7.613,2.782-16.225-0.871-18.354-8.234c-1.984-6.822-0.404-11.161,3.774-15.822
		c10.354-11.484,16.289-18.314,27.467-28.999c16.934-16.185,36.128-20.483,51.063-4.524c21.467,22.959,5.628,60.732,0.064,87.497
		c-0.548,2.653-13.742,63.627-13.742,63.627s17.387-10.645,27.709-14.427c7.628-2.774,16.241,0.887,18.37,8.242
		C306.716,364.537,305.12,368.875,300.942,373.528z M273.169,176.123c-23.886,2.096-44.934-15.564-47.031-39.467
		c-2.08-23.878,15.58-44.934,39.467-47.014c23.87-2.097,44.934,15.58,47.015,39.458
		C314.716,152.979,297.039,174.043,273.169,176.123z" />
            </g>
          </svg>
        </button>
        <button @click="showSection('pays')"
          :class="[sectionVisible === 'pays' ? activeTabColor : inactiveTabColor, 'text-black group flex items-center justify-center rounded-lg px-1 py-1 md:px-3 md:px-3 mx-2 shadow-[-2px_-2px_10px_rgba(255,255,255,1),3px_3px_10px_rgba(0,0,0,0.2)] active:shadow-[inset_-2px_-2px_5px_rgba(255,255,255,0.7),inset_3px_3px_5px_rgba(0,0,0,0.1)]']">
          <span class="sr-only">Settings</span>
          <svg fill="currentColor" version="1.1" id="Layer_1" xmlns="http://www.w3.org/2000/svg"
            xmlns:xlink="http://www.w3.org/1999/xlink" viewBox="0 0 512 512" enable-background="new 0 0 512 512"
            xml:space="preserve" class="h-4 w-4 md:w-6 md:h-6 text-slate-500 group-active:scale-95">
            <path d="M93.8,114.6c-4.7,1.1-1.7,0.9-5.6,1.4C71.5,119.6,83.9,122.8,93.8,114.6z M387.5,121.3c1.2-0.8,5.4-4.9-7.7-8.9
	c0.8,4.1-2.7,3.7-2.7,6c9.7,8.8,13.7,24.1,26.1,27.3C405.6,134.7,392.2,129.3,387.5,121.3z M84.9,111.4c1.5,8.9,8.2-9.4,8.3-15.9
	c-2.6,1.5-5.2,3-7.9,4.2c6.3,3.2,0.8,6.6-6,11.7C65.5,128.6,92.2,98,84.9,111.4z M256,0C114.6,0,0,114.6,0,256
	c0,141.3,114.6,256,256,256c141.4,0,256-114.7,256-256C512,114.6,397.4,0,256,0z M262.8,85.8l1.2,0.4c-4.8,6.2,25,24.3,3.6,25.8
	c-20,5.7,8.4-5.2-7.1-3.3C268.7,97.3,254,97.1,262.8,85.8z M141.4,102.2c-7.2-6-29.8,8.2-21.9,4.8c19.6-7.7,1.3,0.8,5.9,10
	c-4.2,8.7-1.4-8.6-11.8,1.7c-7.5,1.7-25.9,18.7-23.6,13.5c-0.6,8.1-21.9,17.7-24.8,31.2c-7,18.7-1.7-0.7-3-8
	c-10-12.7-28.2,21.5-22.8,35c9.1-16,8.4-1.7,1.8,5.4c6.7,12.3-6.1,28.3,6.6,37.4c5.6,1.3,16.8-18.8,11.9,2.1
	c3.4-18.1,9.4,4.3,19.1-0.7c0.6,9.5,6.5,5.1,7.8,16.6c16.2-1.2,31,26.2,11.7,31.4c2.9-0.8,8.6,4.3,15.2,0.4
	c11.2,8.9,40.7,10,41.5,32c-20.3,9.7-5,36.3-22.6,45.8c-20.2-3-6.9,24.9-15.4,21.7c3.4,20.1-20.4-2.6-11.2,8.5
	c16.9,10.4-7.4,8.3,0.2,15.9c-8.5-1.8,5.3,15.8,7.6,22.3c12.2,19.8-10.5-4.4-17.2-11c-6.4-12.8-21.5-37.3-25.7-57.4
	c-2.4-29.2-25-48.8-30.2-77.3c-5.2-15.9,14.3-41.4,3.8-50.3c-9.1-7.1-5.4-31.4-10.8-44.2c13.5-58.5,56.4-107.8,107.9-137
	c-5.3,3.9,30.3-10.1,26.2-6.7c-1.1,2.5,20.8-9.5,34-11.3c-1.4,0.2-34.3,12-25.2,10.4c-14.1,6.9-1.4,3,5.6-0.5
	c-14,10.3-24.8,7.4-40.7,16.5c-16,4.2-12.7,20.8-24.1,29.1c6.7,1.2,23.5-17.3,33.3-23.8c22.5-10.9-11.4,19.8,10,6.6
	c-7.2,6.7-5.7,17.4-10.1,20.4C148.2,92.1,159.1,97.9,141.4,102.2z M176.4,56.2c-2.3,3.1-5.5,9.8-7.4,5.7c-2.6,1.3-3.6,6.9-8.5,2.4
	c2.9-2.1,5.9-7.1,0.2-4c2.6-2.8,25.8-10.7,24.5-13.7c4.1-2.6,3.7-3.9-1-2.3c-2.4-0.8,5.7-7.6,16.5-8.5c1.5,0,2.1,1-0.6,0.7
	c-16.3,5-9.3,3.6,1.7,0c-4.2,2.4-7.1,3.1-7.8,4.2c11-4-0.6,2.9,1.9,2.4c-3.1,1.6,0.5,2.1-5.5,4.4c1.1-0.9-9.8,6.5-3.3,4.3
	C180.8,57.8,178,57.9,176.4,56.2z M186,70.5c0.2-9.6,14-15.7,12.3-16.2c17-8-5.9,0.3,7.5-6.9c5-0.5,15.6-16.5,30.3-17.5
	c16.2-4.9,8.7,0.3,20.7-4.3l-2.4,2c-2.1,0.3,0.5,4-7.1,9.6c-0.8,8.7-14.5,4.7-7.7,14c-4.4-6.3-11-0.2-2.7,0.4
	c-8.9,6.8-29.6,8-39.5,19.3C191,80.1,185.1,77.2,186,70.5z M257.1,102.5c-6.8,16.4-13.4-2.4-1.4-5.4
	C258.7,98.7,259.9,99.2,257.1,102.5z M231.5,69.7c-2-7.4-0.4-3.5,11.5-7C251.2,68.6,235.7,72.5,231.5,69.7z M417.7,363.2
	c-9.4-16.2,11.4-31.2,18.4-44.8C435.2,334.3,433.2,350,417.7,363.2z M453.1,178.1c-10.2,0.8-19.4,3.2-28.6-2.6
	c-21.2-23.2,3.9,26.2,10.9,6c25.2,9.6-0.4,51-16.3,46.7c-8.9-19.2-19.9-40.3-39.3-49.7c14.9,16.5,22.3,36.8,38.3,51.7
	c1.1,20.8,22.2-7.6,20.9,8.5c2,27.7-31.3,44.3-25.5,72.1c12.4,25.3-23.9,29.9-19.8,52.6c-14.6,16.3-30.2,38.3-56.4,34.8
	c0-13.8-7-25.5-8.6-39.7c-14.2-18,15-37.3-3.1-56.1c-20.9-4.7,4.3-33.5-17.2-30.8c-12.9-12.9-31.8-0.4-50.3-0.3
	c-23.2,2.2-47.1-28.5-36.8-50.2c-8.2-22.6,26-29.2,26.9-49.1c16.4-13.7,39.7-12,61.9-15.2c-1.6,15.9,15.2,16,27.9,21.3
	c7.1-17.2,29.2,2.8,44.3-8.1c5.2-25.4-14.7-10.1-26.1-18.2c-13.8-20.2,29.5-10.4,25-21c-16.8-0.1-7.3-20.7-19.2-9.2
	c10.7,1.9-1.9,10.3-1.6,0.7c-16.2-4.7-0.6,18.4-8.8,20.6c-12.5-5.2-6.6,5.9-5.3,7.6c-5.4,11.7-12-17.2-27.3-16.4
	c-15.2-13.9-6,6.3,7.2,9.6c-2.8,0.8,1.6,12.3-1.9,7.4c-10.9-15-31.6-25-43.9-6.6c-1.3,17.2-36.3,22.1-30.7,2
	c-8.2-20.8,25.4-0.6,22.3-17.2c-21.6-14.3,5.9-9.7,13.2-23.1c16.6,0.5,0.7-13.6,8.5-17.7c-0.8,15.3,12.7,12.4,23.4,9.5
	c-2.6-8.8,6.4-8.5,0.9-15.8c24.8-9.9-18.9,4.6-10.1-17.1c-10.7-7.4-4.5,16.3,0,18.8c0.3,7.3-5.9,16.3-14.4,1
	c-12.4,8.1-11.1-8.2-11.9-6.5c-1.4-6.3,9.4-6.6,9.5-17.6c-0.9-7-0.7-10.7,4.3-11.1c0.4,1,20.5,1.3,27.6,9.6
	c-19.4-3.9-2.9,3.2,5.8,7.2c-9.3-7.3,3.7,0-3.9-8.3c3,0.6-8.3-11.4,3.3-0.9c-6.3-7.5,12.3-5.3,1.3-10.9c16.1,4.5,6.6,0.4-2.9-3.7
	c-26.2-15.6,46.3,21.1,16.7,4.8c18.9,4.1-40.4-14.6-13.4-6.4c-10.3-4.5-0.3-2,9,0.9c-16.7-5.2-41.7-14.9-40.7-15.3
	c5.8,0.4,11.5,1.7,17,3.3c17.1,5.1-4.9-1.2-0.2-1.1C373.8,44,425.3,83.4,456.6,134.9c7.3,7.7,27.2,58.6,16.8,36
	c4.7,18,5.4,37.4,7.9,55.8c-5.2-5.8-11-27.2-16-39.1C463.2,192.2,460.8,181.1,453.1,178.1z" />
          </svg>
        </button>
      </div>
      <div>

        <div
          class="flex p-3 md:px-12 bg-white rounded-lg shadow overflow-auto h-auto max-h-16 sd:max-h-32 md:max-h-96 mt-3 mr-3">
          <div v-if="sectionVisible === 'description'">
            <h3 class="font-mono md:text-lg"> {{ pokemon.description }}</h3>
          </div>
          <div v-if="sectionVisible === 'attaques'">
            <div class="flex">
              <h3 class="font-mono md:text-lg font-bold w-12"> {{ pokemon.attaques1[0].degats }}</h3>
              <NuxtImg :src="pokemon.attaques1[0].types[0].image.url" :alt="pokemon.nom" class="h-8 w-8 rounded" />
              <h3 class="font-mono md:text-lg font-bold"> {{ pokemon.attaques1[0].nom }}</h3>
            </div>
            <div class="flex">
              <h3 class="font-mono md:text-lg font-bold w-12"> {{ pokemon.attaques1[1].degats }}</h3>

              <NuxtImg :src="pokemon.attaques1[1].types[0].image.url" :alt="pokemon.nom" class="h-8 w-8 rounded" />
              <h3 class="font-mono md:text-lg font-bold"> {{ pokemon.attaques1[1].nom }}</h3>
            </div>
            <div class="flex">
              <h3 class="font-mono md:text-lg font-bold w-12"> {{ pokemon.attaques1[2].degats }}</h3>
              <NuxtImg :src="pokemon.attaques1[2].types[0].image.url" :alt="pokemon.nom" class="h-8 w-8 rounded" />
              <h3 class="font-mono md:text-lg font-bold"> {{ pokemon.attaques1[2].nom }}</h3>
            </div>
            <div class="flex">
              <h3 class="font-mono md:text-lg font-bold w-12"> {{ pokemon.attaques1[3].degats }}</h3>

              <NuxtImg :src="pokemon.attaques1[3].types[0].image.url" :alt="pokemon.nom" class="h-8 w-8 rounded" />
              <h3 class="font-mono md:text-lg font-bold"> {{ pokemon.attaques1[3].nom }}</h3>
            </div>

          </div>
          <div v-if="sectionVisible === 'localisation'">
            <h3 class="font-mono md:text-lg"> Ce Pokémon peut être trouvé sur la {{ pokemon.routes[0].nom }}</h3>
          </div>
          <div v-if="sectionVisible === 'informations'">
            <h3 class="font-mono md:text-lg"> Poids : {{ pokemon.poids }}kg</h3>
            <h3 class="font-mono md:text-lg"> Taille : {{ pokemon.taille }}cm</h3>
            <h3 class="font-mono md:text-lg"> PV de base : {{ pokemon.pointsDeVie }}PV</h3>
            <h3 class="font-mono md:text-lg"> Taux mâle : {{ pokemon.genre }}%</h3>
            <h3 class="font-mono md:text-lg"> Taux femelle : {{ 100 - pokemon.genre }}%</h3>



          </div>
          <div v-if="sectionVisible === 'pays'">
            <h3 class="font-mono md:text-lg"> Ce Pokemon est originaire de : <br>{{ pokemon.pays }}</h3>
          </div>
        </div>
      </div>
      <div class=" h-full flex flex-col justify-center items-center">
        <h2 class="font-mono text-3xl md:text-7xl mb-4"> {{ pokemon.nom }}</h2>
        <div class="flex">
          <NuxtImg :src="pokemon.types[0].image.url" :alt="pokemon.nom" class="h-16 w-16 rounded-full" />
          <NuxtImg :src="pokemon.types[1].image.url" :alt="pokemon.nom" class="h-16 w-16 rounded-full" />
        </div>
      </div>
    </div>
  </div>



  <div v-else>
    <li>Loading...</li>
  </div>
</template>