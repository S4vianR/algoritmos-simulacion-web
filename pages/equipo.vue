<template>
  <div>
    <NavBar />
    <MainSlot>
      Somos un equpo de estudiantes del Instituto Tecnológico de Ciudad Juárez
    </MainSlot>
  </div>
</template>

<script lang="ts" setup>
import MainSlot from '~/layouts/MainSlot.vue';

// Set page title
useHead({
    title: "DataDynamos | Equipo",
  });
</script>

<style>

</style>