<script setup>
const query = gql`
  query Pokemon($slug: String!) {
    pokemon(where: { slug: $slug }) {
      id
      nom
      slug
      description
      taille
      poids
      pv
      couleur {
        hex
      }
      createdAt
      publishedAt
      updatedAt
      stage
      image {
        url(
          transformation: {
            image: { resize: { fit: crop, height: 1024, width: 1024 } }
            document: { output: { format: webp } }
          }
        )
      }
      types {
        id
        nom
        image {
          url
        }
      }
      attaques {
        id
        nom
        description
        degat
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
</script>

<template>
  <div v-if="pokemon" class="w-full m-auto w-2/3 bg-[#f3f3f4] p-5 rounded-lg">
    <div class="flex flex-row">
      <div class="w-1/3 m-10">
        <h2 class="text-6xl text-center tracking-widest mb-10">{{ pokemon.nom }}</h2>
        <NuxtImg
          :src="pokemon.image.url"
          :alt="pokemon.nom"
          class="border-4 border-[#2b4967] rounded-lg transition-all duration-300 ease-in-out hover:scale-110"
        />
      </div>
      <div class="w-2/3 mt-10">
        <div class="flex flex-row">
          <div
            class="bg-[#7f9eb6] text-white p-4 rounded-lg w-full flex justify-between"
          >
            <p>{{ pokemon.taille }} centimètres</p>
            <p>{{ pokemon.poids }} kilos</p>
            <p>
              <span
                :style="{ backgroundColor: pokemon.couleur.hex }"
                class="m-auto inline-block ml-2 mr-2 w-20 h-7 border border rounded text-center"
                >{{ pokemon.couleur.hex }}</span
              >
            </p>
            <div v-if="pokemon.types" class="">
              <p v-for="type in pokemon.types" :key="type.nom">
                {{ type.nom }}
              </p>
            </div>
            <p>{{ pokemon.pv }} pvs</p>
          </div>
        </div>
        <p class="pt-2 italic">{{ pokemon.description }}</p>
        <div v-if="pokemon.attaques" class="mt-5 border-t-2 border-[#7f9eb6]">
          <div
            v-if="pokemon.attaques"
            class="mt-5 bg-[#7f9eb6] shadow-lg rounded-lg p-4"
          >
            <div
              v-for="attaque in pokemon.attaques"
              :key="attaque.nom"
              class="mt-2 p-2 bg-white bg-opacity-80 rounded hover:bg-opacity-100 hover:shadow-md transition duration-300 ease-in-out flex justify-between items-start"
            >
              <div>
                <span class="font-bold uppercase text-blue-800">{{
                  attaque.nom
                }}</span>
                <span class="ml-3 italic text-gray-600">{{
                  attaque.description
                }}</span>
              </div>
              <span class="text-sm font-semibold text-blue-800">
                {{ attaque.degat }} PV</span
              >
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
  <div v-else>
    <li>Loading...</li>
  </div>
</template>
