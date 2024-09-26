<template>
  <div>
    <Header class="w-full h-[7svh]">
      <nav class="p-5  flex items-center justify-center gap-12">
        <h1 class="text-lg font-semibold">
          <nuxt-link to="/">DataDynamos</nuxt-link>
        </h1>
        <ol class="flex items-center justify-center gap-4">
          <li class="transition-transform hover:-translate-y-1">
            <nuxt-link to="/">Inicio</nuxt-link>
          </li>
          <li class="transition-transform hover:-translate-y-1">
            <nuxt-link to="/equipo">Equipo</nuxt-link>
          </li>
        </ol>
      </nav>
    </Header>
  </div>
</template>

<script lang="ts" setup>

</script>