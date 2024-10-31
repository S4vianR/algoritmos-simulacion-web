<template>
  <div class="flex flex-col gap-2 justify-center items-center">
    <!-- Tabla de resultados -->
    <div id="contenedorResultados" class="flex flex-col justify-center items-center gap-2">
      <h1 class="self-center font-bold">Estudio de tiempos</h1>
      <div class="flex flex-row justify-center items-center gap-2">
        <button @click="exportarCSV" class="bg-yellow-500 text-white px-4 py-2 rounded-md">Exportar a CSV</button>
      <button @click="exportarJSON" class="bg-yellow-500 text-white px-4 py-2 rounded-md">Exportar a JSON</button>
      </div>
      <table class="w-[48rem] border border-yellow-500">
        <thead>
          <tr>
            <th class="border p-2 border-yellow-500 bg-yellow-200 text-center">Descripción</th>
            <th class="border p-2 border-yellow-500 bg-yellow-200 text-center" colspan="6">Tiempo observado</th>
          </tr>
          <tr>  
            <th class="border p-2 border-yellow-500 bg-yellow-200 text-center">Elemento</th>
            <th class="border p-2 border-yellow-500 bg-yellow-200 text-center">1</th>
            <th class="border p-2 border-yellow-500 bg-yellow-200 text-center">2</th>
            <th class="border p-2 border-yellow-500 bg-yellow-200 text-center">3</th>
            <th class="border p-2 border-yellow-500 bg-yellow-200 text-center">4</th>
            <th class="border p-2 border-yellow-500 bg-yellow-200 text-center">5</th>
            <th class="border p-2 border-yellow-500 bg-yellow-200 text-center">Σ</th>
          </tr>
        </thead>
        <tbody>
          <!-- Filas de datos -->
          <tr v-for="(elemento, index) in elementosNombres" :key="index">
            <td class="border p-2 border-yellow-500 bg-yellow-100 text-start">
              <!-- Input para escribir los elementos -->
              <input :id="`elemento-${index}`" type="text" v-model="elementosNombres[index]"
                class="w-24 p-1 border border-yellow-200 bg-yellow-100 outline-yellow-500 text-black text-start rounded-md">
            </td>
            <td v-for="(tiempo, j) in elementosTiempos[index]" :key="`elemento-${index}-tiempo-${j}`"
              class="border p-2 border-yellow-500 bg-yellow-100 text-end">
              <input :id="`elemento-${index}-tiempo-${j - 1}`" type="number" step="0.01" v-model="elementosTiempos[index][j]"
                class="w-24 p-1 border border-yellow-200 bg-yellow-100 outline-yellow-500 text-black text-end rounded-md"
                @input="actualizarSumas">
            </td>
            <!-- Columna de suma por elemento -->
            <td class="border p-2 border-yellow-500 bg-yellow-100 text-end font-bold">
              {{ sumasTiempos[index]?.toFixed(2) || "0.00" }}
            </td>
          </tr>
          <!-- Fila de promedios -->
          <tr>
            <td class="border p-2 border-yellow-500 bg-yellow-200 text-start font-bold">PROMEDIO</td>
            <td v-for="promedio in sumasTiemposPromedio" :key="`promedio-${promedio}`"
              class="border p-2 border-yellow-500 bg-yellow-100 text-end">
              {{ promedio.toFixed(4) }}
            </td>
            <!-- Otro td solo para rellenar -->
            <td class="border p-2 border-yellow-500 bg-yellow-100 text-end"></td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script lang="ts" setup>
// Variables
const elementos = ref<number>();
const veces = ref<number>();
const elementosNombres = reactive<string[]>([]);
const elementosTiempos = reactive<number[][]>([]);
const sumasTiempos = reactive<number[]>([]);
const sumasTiemposPromedio = reactive<number[]>([]);

const actualizarSumas = () => {
  for (let i = 0; i < (elementos.value ?? 0); i++) {
    let tiempos: number[] = [];
    for (let j = 0; j < (veces.value ?? 0); j++) {
      tiempos.push(elementosTiempos[i][j] || 0);
    }
    sumasTiempos[i] = parseFloat(tiempos.reduce((a, b) => a + b).toFixed(4));
  }

  for (let j = 0; j < (veces.value ?? 0); j++) {
    let suma = 0;
    for (let i = 0; i < (elementos.value ?? 0); i++) {
      suma += elementosTiempos[i][j] || 0;
    }
    sumasTiemposPromedio[j] = parseFloat((suma / (elementos.value ?? 1)).toFixed(4));
  }
}

// Función para exportar a CSV
const exportarCSV = () => {
  let csvContent = "data:text/csv;charset=utf-8,";

  // Encabezado
  csvContent += "Descripción,Tiempo 1,Tiempo 2,Tiempo 3,Tiempo 4,Tiempo 5,Suma\n";

  // Filas de datos
  elementosNombres.forEach((nombre, index) => {
    const tiempos = elementosTiempos[index].map((t) => t.toFixed(2)).join(",");
    const suma = sumasTiempos[index]?.toFixed(2) || "0.00";
    csvContent += `${nombre},${tiempos},${suma}\n`;
  });

  // Fila de promedios
  csvContent += `PROMEDIO,${sumasTiemposPromedio.map((p) => p.toFixed(4)).join(",")}\n`;

  // Descargar archivo CSV
  const encodedUri = encodeURI(csvContent);
  const link = document.createElement("a");
  link.setAttribute("href", encodedUri);
  link.setAttribute("download", "estudio_tiempos.csv");
  document.body.appendChild(link);
  link.click();
  document.body.removeChild(link);
};

// Función para exportar a JSON
const exportarJSON = () => {
  const data = elementosNombres.map((nombre, index) => ({
    descripcion: nombre,
    tiempos: elementosTiempos[index],
    suma: sumasTiempos[index]?.toFixed(2) || "0.00"
  }));
  const jsonData = {
    elementos: data,
    promedios: sumasTiemposPromedio
  };

  // Convertir a JSON y descargar
  const dataStr = "data:text/json;charset=utf-8," + encodeURIComponent(JSON.stringify(jsonData));
  const link = document.createElement("a");
  link.setAttribute("href", dataStr);
  link.setAttribute("download", "estudio_tiempos.json");
  document.body.appendChild(link);
  link.click();
  document.body.removeChild(link);
};

onMounted(() => {
  elementos.value = 5;
  veces.value = 5;

  for (let i = 0; i < (elementos.value ?? 0); i++) {
    elementosNombres[i] = `Elemento ${i + 1}`;
    elementosTiempos[i] = [];
    for (let j = 0; j < (veces.value ?? 0); j++) {
      elementosTiempos[i][j] = 0.0;
    }
  }

  actualizarSumas();
})
</script>

<style></style>
