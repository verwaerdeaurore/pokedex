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
  <div v-if="pokemon" class="w-full m-auto bg-[#f3f3f4] p-5 rounded-lg">
    <div class="text-center mb-10">
      <h2 class="text-6xl tracking-widest">{{ pokemon.nom }}</h2>
    </div>
    <div class="flex flex-row">
      <div class="w-1/3 m-10">
        <NuxtImg
          :src="pokemon.image.url"
          :alt="pokemon.nom"
          class="border-4 border-[#2b4967] rounded-lg transition-all duration-300 ease-in-out hover:scale-110"
        />
      </div>
      <div class="w-2/3 mt-20">
        <div class="flex flex-row">
          <div
            class="bg-[#7f9eb6] text-white p-4 rounded-lg w-full flex justify-between"
          >
            <div>Taille : {{ pokemon.taille }} centimètres</div>
            <div>Poids : {{ pokemon.poids }} kilos</div>
            <div>Couleur : <span :style="{ backgroundColor: pokemon.couleur.hex, }" class=" m-auto inline-block ml-2 mr-2 w-20 h-7 border border rounded text-center">{{ pokemon.couleur.hex }}</span></div>
            <div>Points de vie : {{ pokemon.pv }} pvs</div>

          </div>
        </div>
        <p class="pt-2 italic">{{ pokemon.description }}</p>
      </div>
    </div>
  </div>
  <div v-else>
    <li>Loading...</li>
  </div>
</template>
