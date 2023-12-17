<script setup>
const query = gql`
  query Pokemons {
    pokemons(orderBy: id_ASC, first:100) {
      createdAt
      description
      id
      nom
      publishedAt
      slug
      updatedAt
      image {
        url(
          transformation: {
            document: { output: { format: webp } }
            image: { resize: { fit: crop, height: 400, width: 400 } }
          }
        )
      }
    }
  }
`;

const pokemons = ref();
const { data } = await useAsyncQuery(query);
console.log(data.value);
pokemons.value = data.value.pokemons;
</script>

<template>
  <ul
    v-if="pokemons"
    class="grid gap-8 grid-cols-2 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-5 xl:grid-cols-6 p-4"
  >
    <li 
      v-for="pokemon in pokemons" 
      :key="pokemon.id" 
      class="bg-white rounded-lg shadow-lg overflow-hidden transform hover:scale-105 transition-transform duration-300"
    >
      <NuxtLink :to="`/pokemon/${pokemon.slug}`" class="block">
        <div class="relative">
          <NuxtImg 
            :src="pokemon.image.url" 
            :alt="pokemon.nom" 
            class="w-full h-64 object-cover"
          />
          <div class="absolute bottom-0 left-0 w-full bg-gradient-to-t from-gray-900 to-transparent py-2 px-4">
            <h2 class="text-lg font-semibold text-white truncate">
              {{ pokemon.nom }}
            </h2>
          </div>
        </div>
      </NuxtLink>
    </li>
  </ul>
  <ul v-else>
    <li class="text-white">Loading...</li>
  </ul>
</template>





