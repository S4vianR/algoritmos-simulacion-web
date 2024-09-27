<template>
  <div>
    <form @submit="formSubmit" method="post"
      class="p-2 border border-yellow-500 rounded-sm w-96 flex flex-col gap-2 justify-center items-end">
      <h1 class="self-center font-bold">
        Algoritmo congruencial cuadrático
      </h1>
      <div class="flex flex-row justify-end items-center gap-2">
        <label for="semilla">Semilla (x0):</label>
        <input type="number" name="semilla" id="semilla" min="0"
          class="w-24 border border-yellow-200 bg-yellow-100 outline-yellow-500 text-black text-end rounded-md" />
      </div>
      <div class="flex flex-row justify-end items-center gap-2">
        <label for="numPar">Número par (a):</label>
        <input type="number" name="numPar" id="numPar" min="0"
          class="w-24 border border-yellow-200 bg-yellow-100 outline-yellow-500 text-black text-end rounded-md" />
      </div>
      <div class="flex flex-row justify-end items-center gap-2">
        <label for="numImpar">Número impar (c):</label>
        <input type="number" name="numImpar" id="numImpar" min="0"
          class="w-24 border border-yellow-200 bg-yellow-100 outline-yellow-500 text-black text-end rounded-md" />
      </div>
      <div class="flex flex-row justify-end items-center gap-2">
        <label for="coeficienteLineal"> Coeficiente lineal (b):</label>
        <input type="number" name="coeficienteLineal" id="coeficienteLineal" min="0"
          class="w-24 border border-yellow-200 bg-yellow-100 outline-yellow-500 text-black text-end rounded-md" />
      </div>
      <div class="flex flex-row justify-end items-center gap-2">
        <label for="modulo">Modulo (m):</label>
        <input type="number" name="modulo" id="modulo" min="0"
          class="w-24 border border-yellow-200 bg-yellow-100 outline-yellow-500 text-black text-end rounded-md" />
      </div>
      <button type="submit"
        class="w-24 border border-yellow-200 bg-yellow-100 outline-yellow-500 rounded-md self-center">
        Enviar
      </button>
    </form>
    <div>
      <p v-if="variablesVacias">
        {{ mensajeInicial }}
      </p>
      <textarea name="algoritmoLinalLog" id="algoritmoLinalLog" v-else-if="!variablesVacias" cols="40" rows="16"
        readonly class="mt-3 border border-yellow-200 bg-yellow-100 text-black rounded-md resize-none">
        {{ mensajeAux }}
      </textarea>
    </div>
  </div>
</template>

<script lang="ts" setup>
// Variables
const semilla = ref<number>();
const numPar = ref<number>();
const numImpar = ref<number>();
const coeficienteLineal = ref<number>();
const modulo = ref<number>();
const variablesVacias = ref<boolean>();
const mensajeAux = ref<string>();
const mensajeInicial = ref<string>();

let semillaCheck: HTMLInputElement;
let numParCheck: HTMLInputElement;
let numImparCheck: HTMLInputElement;
let coeficienteLinealCheck: HTMLInputElement;
let moduloCheck: HTMLInputElement;

// Metodos
const formSubmit = (e: any) => {
  e.preventDefault();
  semilla.value = Number(semillaCheck.value);
  numPar.value = Number(numParCheck.value);
  numImpar.value = Number(numImparCheck.value);
  coeficienteLineal.value = Number(coeficienteLinealCheck.value);

  modulo.value = Number(moduloCheck.value);

  if (semilla.value != 0 && numPar.value != 0 && numImpar.value != 0 && coeficienteLineal.value != 0 && modulo.value != 0) {
    // Resetear los checks y añadir border-yellow-200 bg-yellow-100
    semillaCheck.classList.remove("border-red-500", "bg-red-100", "outline-red-500");
    numParCheck.classList.remove("border-red-500", "bg-red-100", "outline-red-500");
    numImparCheck.classList.remove("border-red-500", "bg-red-100", "outline-red-500");
    coeficienteLinealCheck.classList.remove("border-red-500", "bg-red-100", "outline-red-500");
    moduloCheck.classList.remove("border-red-500", "bg-red-100", "outline-red-500");

    semillaCheck.classList.add("border-yellow-200", "bg-yellow-100", "outline-yellow-500");
    numParCheck.classList.add("border-yellow-200", "bg-yellow-100", "outline-yellow-500");
    numImparCheck.classList.add("border-yellow-200", "bg-yellow-100", "outline-yellow-500");
    coeficienteLinealCheck.classList.add("border-yellow-200", "bg-yellow-100", "outline-yellow-500");
    moduloCheck.classList.add("border-yellow-200", "bg-yellow-100", "outline-yellow-500");

    variablesVacias.value = false;

    // Logica para hacer el algoritmo lineal
    /*
      def algoritmoCongruencial(x0, a, b ,c, m):
    i = 1
    while i <= 10:
        res = ((a*(x0**2))+(b*x0)+c)%m
        print(f"Iteracion no.{i}: {res}")
        i += 1
        x0 = res  # Actualizar x0 para el siguiente número
    print("\n")
    */

    // mensajeInicial.value = `Número par: ${numPar.value}, Número impar: ${numImpar.value}, Coeficiente lineal: ${coeficienteLineal.value}, Modulo: ${modulo.value}`;
    let i = 1;
    let res = 0;
    mensajeAux.value = 'Iteraciones'
    mensajeAux.value += `\nSemilla: ${semilla.value}`;
    while (i <= 150 && semilla.value != 0) {
      if (coeficienteLineal.value !== undefined) {
        res = (((numPar.value * (semilla.value ** 2)) + (coeficienteLineal.value * semilla.value) + numImpar.value) % modulo.value);
      } else {
        // Handle the case where coeficienteLineal.value is undefined
        res = 0; // or any other default value or error handling
      }
      mensajeAux.value += `\nIteracion no.${i}: ${res}`;
      i += 1;
      semilla.value = res;
    }
  } else if (numPar.value % 2 !== 0 && numImpar.value === 0) {
    numParCheck.classList.remove("border-yellow-200", "bg-yellow-100", "outline-yellow-500");
    numImparCheck.classList.remove("border-yellow-200", "bg-yellow-100", "outline-yellow-500");

    numParCheck.classList.add("border-red-500", "bg-red-100", "outline-red-500");
    numImparCheck.classList.add("border-red-500", "bg-red-100", "outline-red-500");

    mensajeInicial.value = "El número par debe ser par y el número impar debe ser impar";
  } else {
    // Resetear los checks y añadir border-red-500 bg-red-100
    semillaCheck.classList.remove("border-yellow-200", "bg-yellow-100", "outline-yellow-500");
    numParCheck.classList.remove("border-yellow-200", "bg-yellow-100", "outline-yellow-500");
    numImparCheck.classList.remove("border-yellow-200", "bg-yellow-100", "outline-yellow-500");
    coeficienteLinealCheck.classList.remove("border-yellow-200", "bg-yellow-100", "outline-yellow-500");
    moduloCheck.classList.remove("border-yellow-200", "bg-yellow-100", "outline-yellow-500");

    semillaCheck.classList.add("border-red-500", "bg-red-100", "outline-red-500");
    numParCheck.classList.add("border-red-500", "bg-red-100", "outline-red-500");
    numImparCheck.classList.add("border-red-500", "bg-red-100", "outline-red-500");
    coeficienteLinealCheck.classList.add("border-red-500", "bg-red-100", "outline-red-500");
    moduloCheck.classList.add("border-red-500", "bg-red-100", "outline-red-500");
  }
};

onMounted(() => {
  variablesVacias.value = true;
  mensajeAux.value = "";
  mensajeInicial.value = "Favor de rellenar los valores solicitados";

  semillaCheck = document.getElementById("semilla") as HTMLInputElement;
  numParCheck = document.getElementById("numPar") as HTMLInputElement;
  numImparCheck = document.getElementById("numImpar") as HTMLInputElement;
  coeficienteLinealCheck = document.getElementById("coeficienteLineal") as HTMLInputElement;
  moduloCheck = document.getElementById("modulo") as HTMLInputElement;

  semillaCheck.classList.add("border-yellow-200", "bg-yellow-100");
  numParCheck.classList.add("border-yellow-200", "bg-yellow-100");
  numImparCheck.classList.add("border-yellow-200", "bg-yellow-100");
  coeficienteLinealCheck.classList.add("border-yellow-200", "bg-yellow-100");
  moduloCheck.classList.add("border-yellow-200", "bg-yellow-100");

  semillaCheck.focus();
});
</script>
