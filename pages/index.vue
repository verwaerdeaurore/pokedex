<script setup>
const query = gql`
  query Pokemons {
    pokemons(orderBy: id_ASC, first: 100) {
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
const searchQuery = ref('');
const { data } = await useAsyncQuery(query);
console.log(data.value);
pokemons.value = data.value.pokemons;


if (data.value) {
  pokemons.value = data.value.pokemons;
}
const filteredPokemons = computed(() => {
  return pokemons.value.filter(pokemon =>
    pokemon.nom.toLowerCase().includes(searchQuery.value.toLowerCase())
  );
});
</script>

<template>
  <input
      type="text"
      v-model="searchQuery"
      placeholder="Rechercher un Pokémon"
      class="w-full p-2 border border-gray-300 rounded-md"
    />
  <ul
      v-if="filteredPokemons"
      class="grid gap-8 grid-cols-2 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-5 xl:grid-cols-6 2xl:grid-cols-6 mt-4"
    >
      <li v-for="pokemon in filteredPokemons" :key="pokemon.id">
      <NuxtLink :to="`/pokemon/${pokemon.slug}`" class="group block">
        <div
          class="relative aspect-w-1 aspect-h-1 group-hover:shadow-lg transition-all duration-300"
        >
          <NuxtImg
            :src="pokemon.image.url"
            :alt="pokemon.nom"
            class="object-cover w-full h-full rounded-md"
          />
          <div
            class="absolute inset-0 bg-black bg-opacity-40 opacity-0 group-hover:opacity-100 transition-opacity duration-300 flex items-center justify-center"
          >
            <h2 class="text-white text-lg font-semibold">{{ pokemon.nom }}</h2>
          </div>
        </div>
      </NuxtLink>
    </li>
  </ul>
  <ul v-else>
    <li class="text-white">Loading...</li>
  </ul>
</template>
