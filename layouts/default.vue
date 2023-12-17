<template>
  <div class="min-h-screen bg-[#c2ddef]">
    <nav
      class="w-full h-24 flex items-center justify-center mb-10 bg-[#7f9eb6]"
    >
      <div class="flex items-center justify-center w-full">
        <img src="../images/logo.png" alt="Logo" class="h-20 mr-3" />
        <NuxtLink to="/">
          <h1 class="text-5xl text-white uppercase">Pokedex</h1>
        </NuxtLink>
        <img src="../images/logo.png" alt="Logo" class="h-20 ml-3" />
      </div>
    </nav>
    <div class="container mx-auto px-3">
      <slot />
    </div>
  </div>
</template>
