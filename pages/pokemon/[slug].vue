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
  <div v-if="pokemon" class="max-w-lg m-auto">
    <NuxtImg :src="pokemon.image.url" :alt="pokemon.nom" />
    <h2 class="text-3xl text-center">{{ pokemon.nom }}</h2>
    <p class="pt-5">{{ pokemon.taille }} centimètres</p>
    <p class="pt-5">{{ pokemon.poids }} kilos</p>
    <p class="pt-5">{{ pokemon.description }}</p>
  </div>

  <div v-else>
    <li>Loading...</li>
  </div>
</template>
