<template>
  <div class="flex flex-col justify-center items-center">
    <form @submit="formSubmit" method="post"
      class="p-2 border border-yellow-500 rounded-sm w-96 flex flex-col gap-2 justify-center items-end">
      <h1 class="self-center font-bold">
        Cuadrados medios
      </h1>
      <div class="flex flex-row justify-end items-center gap-2">
        <label for="semilla">Semilla (xo):</label>
        <input type="number" name="semilla" id="semilla" min="1"
          class="w-24 border border-yellow-200 bg-yellow-100 outline-yellow-500 text-black text-end rounded-md" />
      </div>
      <button type="submit"
        class="w-24 border border-yellow-200 bg-yellow-100 outline-yellow-500 rounded-md self-center">
        Enviar
      </button>
    </form>
    <div class="flex flex-col justify-center items-center">
      <p v-if="variablesVacias">
        {{ mensajeInicial }}
      </p>
      <textarea name="algoritmoLinalLog" id="algoritmoLinalLog" v-else-if="!variablesVacias" readonly cols="46"
        rows="16" class="p-2 mt-3 border border-yellow-200 bg-yellow-100 text-black rounded-md resize-none text-start">
        {{ mensajeAux }}
      </textarea>
    </div>
  </div>
</template>

<script lang="ts" setup>
// Variables
const semilla = ref<number>();
const variablesVacias = ref<boolean>();
const mensajeAux = ref<string>("");
const mensajeInicial = ref<string>();

let semillaInput: HTMLInputElement;

// Metodos
const formSubmit = (e: any) => {
  e.preventDefault();
  semilla.value = Number(semillaInput.value);

  if (semilla.value !== 0) {
    // Resetear los checks y añadir border-yellow-200 bg-yellow-100
    semillaInput.classList.remove("border-red-500", "bg-red-100", "outline-red-500");

    semillaInput.classList.add("border-yellow-200", "bg-yellow-100", "outline-yellow-500");

    variablesVacias.value = false;

    // Logica para hacer el algoritmo de cuadrados medios
    mensajeAux.value = `Semilla: ${semilla.value}\n`;

    let semillaStr = semilla.value.toString();
    let longitudSemilla = semillaStr.length;

    // Tomar los valores del medio del resultado de la multiplicación, xn es el medio de la semilla al cuadrado y rn es el número aleatorio
    let semillaCuadrado: number;
    let xn: number = semilla.value;
    let rn: number;
    let i: number = 1;

    while (i <= 150 && xn !== 0) {
      semillaCuadrado = Math.pow(xn, 2);
      semillaStr = semillaCuadrado.toString();
      longitudSemilla = semillaStr.length;

      let valoresMedio;
      if (longitudSemilla < 4) {
        valoresMedio = semillaStr;
      } else {
        valoresMedio = semillaStr.substring((longitudSemilla - 4) / 2, (longitudSemilla + 4) / 2);
      }
      xn = Number(valoresMedio);

      rn = xn / 10000;

      semilla.value = xn;

      mensajeAux.value += `Iteración no.${i}: ${xn}, Pseudoaleatorio: ${rn}\n`;
      i++;
    }

  } else {
    // Resetear los checks y añadir border-red-500 bg-red-100
    semillaInput.classList.remove("border-yellow-200", "bg-yellow-100", "outline-yellow-500");

    semillaInput.classList.add("border-red-500", "bg-red-100", "outline-red-500");
  }
};

onMounted(() => {
  variablesVacias.value = true;
  mensajeAux.value = "";
  mensajeInicial.value = "Favor de rellenar los valores solicitados";

  semillaInput = document.getElementById("semilla") as HTMLInputElement;

  semillaInput.classList.add("border-yellow-200", "bg-yellow-100", "outline-yellow-500");

  semillaInput.focus();
});
</script>