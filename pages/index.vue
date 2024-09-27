<template>
  <div>
    <NavBar />
    <MainSlot>
      <section class="flex flex-col gap-1 justify-start items-center">
        <div class="flex gap-5">
          <button
            class="p-1 border border-yellow-200 bg-yellow-200 hover:bg-yellow-300 rounded-md"
            @click="setResultado('algoL')"
          >
            Algortimo Lineal
          </button>
          <button
            class="p-1 border border-yellow-200 bg-yellow-200 hover:bg-yellow-300 rounded-md"
            @click="setResultado('algoC')"
          >
            Algoritmo Cuadrático
          </button>
          <button
            class="p-1 border border-yellow-200 bg-yellow-200 hover:bg-yellow-300 rounded-md"
            @click="setResultado('cuadMed')"
          >
            Cuadrados medios
          </button>
          <button
            class="p-1 border border-yellow-200 bg-yellow-200 hover:bg-yellow-300 rounded-md"
            @click="setResultado('varProm')"
          >
            Varianza/Promedio
          </button>
        </div>
      </section>
      <section class="flex flex-col gap-1 justify-start items-center">
        <div v-if="resultado === 'empty'">Seleccione un algoritmo</div>
        <AlgoritmoLineal v-else-if="resultado === 'algoL'" />
        <AlgoritmoCuadratico v-else-if="resultado === 'algoC'" />
        <CuadradosMedios v-else-if="resultado === 'cuadMed'" />
        <VarianzaPromedio v-else-if="resultado === 'varProm'" />
      </section>
    </MainSlot>
  </div>
</template>

<script lang="ts" setup>
// Layouts
import MainSlot from "~/layouts/MainSlot.vue";

// Reactive
const resultado = ref<string>("");

// Methods
const setResultado = (value: string) => {
  resultado.value = value;
};

onMounted(() => {
  resultado.value = "empty";

  // Set page title
  useHead({
    title: "DataDynamos | Inicio",
  });
});
</script>

<style></style>
