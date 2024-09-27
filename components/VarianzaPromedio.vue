<template>
  <div>
    <form @submit="formSubmit" method="post"
      class="p-2 border border-yellow-500 rounded-sm w-96 flex flex-col gap-2 justify-center items-end">
      <h1 class="self-center font-bold">
        Media y Varianza
      </h1>
      <div class="flex flex-row justify-end items-center gap-2">
        <label for="numerosGenerar">Números a generar (n):</label>
        <input type="number" name="numerosGenerar" id="numerosGenerar" min="1"
          class="w-24 border border-yellow-200 bg-yellow-100 outline-yellow-500 text-black text-end rounded-md" />
      </div>
      <button type="submit" class="w-24 border border-yellow-200 bg-yellow-100 outline-yellow-500 rounded-md self-center">
        Enviar
      </button>
    </form>
    <div class="flex flex-col justify-center items-center">
      <p v-if="variablesVacias">
        {{ mensajeInicial }}
      </p>
      <textarea name="algoritmoLinalLog" id="algoritmoLinalLog" v-else-if="!variablesVacias"
        readonly class="p-2 mt-3 w-72 h-24 border border-yellow-200 bg-yellow-100 text-black rounded-md resize-none text-start">
        {{ mensajeAux }}
      </textarea>
    </div>
  </div>
</template>

<script lang="ts" setup>
// Variables
const numerosGenerar = ref<number>();
const variablesVacias = ref<boolean>();
const mensajeAux = ref<string>("");
const mensajeInicial = ref<string>();

let numerosGenerarInput: HTMLInputElement;

// Metodos
const formSubmit = (e: any) => {
  e.preventDefault();
  numerosGenerar.value = Number(numerosGenerarInput.value);

  let arrayNumeros = [] as number[];

  if (numerosGenerar.value !== 0) {
    // Resetear los checks y añadir border-yellow-200 bg-yellow-100
    numerosGenerarInput.classList.remove("border-red-500", "bg-red-100", "outline-red-500");

    numerosGenerarInput.classList.add("border-yellow-200", "bg-yellow-100", "outline-yellow-500");

    variablesVacias.value = false;

    // Logica para hacer el algoritmo lineal
    mensajeAux.value = `Números a generar: ${numerosGenerar.value}\n`;

    // Rellenar el arreglo con números aleatorios
    for (let i = 0; i < numerosGenerar.value; i++) {
      arrayNumeros.push(Math.floor(Math.random() * 100));
    }

    // Calcular la media
    let media = arrayNumeros.reduce((a, b) => a + b) / arrayNumeros.length;

    // Calcular la varianza
    let varianza = arrayNumeros.reduce((a, b) => a + Math.pow(b - media, 2), 0) / arrayNumeros.length;

    // Mostrar la media y la varianza
    mensajeAux.value += `Media: ${media}\nVarianza: ${varianza}`;
  } else {
    // Resetear los checks y añadir border-red-500 bg-red-100
    numerosGenerarInput.classList.remove("border-yellow-200", "bg-yellow-100", "outline-yellow-500");

    numerosGenerarInput.classList.add("border-red-500", "bg-red-100", "outline-red-500");
  }
};

onMounted(() => {
  variablesVacias.value = true;
  mensajeAux.value = "";
  mensajeInicial.value = "Favor de rellenar los valores solicitados";

  numerosGenerarInput = document.getElementById("numerosGenerar") as HTMLInputElement;

  numerosGenerarInput.classList.add("border-yellow-200", "bg-yellow-100", "outline-yellow-500");

  numerosGenerarInput.focus();
});
</script>