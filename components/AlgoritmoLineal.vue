<template>
  <div>
    <form @submit="formSubmit" method="post"
      class="p-2 border border-yellow-500 rounded-sm w-full flex flex-col gap-2 justify-center items-end">
      <h1 class="self-center font-bold">
        Algoritmo lineal
      </h1>
      <div class="flex flex-row justify-end items-center gap-2">
        <label for="semilla">Semilla (x0):</label>
        <input type="number" name="semilla" id="semilla" min="0"
          class="w-24 border border-yellow-200 bg-yellow-100 outline-yellow-500 text-black text-end rounded-md" />
      </div>
      <div class="flex flex-row justify-end items-center gap-2">
        <label for="constante">Constante (a):</label>
        <input type="number" name="constante" id="constante" min="0"
          class="w-24 border border-yellow-200 bg-yellow-100 outline-yellow-500 text-black text-end rounded-md" />
      </div>
      <div class="flex flex-row justify-end items-center gap-2">
        <label for="constanteC">Constante multiplicativa (c):</label>
        <input type="number" name="constanteC" id="constanteC" min="0"
          class="w-24 border border-yellow-200 bg-yellow-100 outline-yellow-500 text-black text-end rounded-md" />
      </div>
      <div class="flex flex-row justify-end items-center gap-2">
        <label for="modulo">Modulo (m):</label>
        <input type="number" name="modulo" id="modulo" min="0"
          class="w-24 border border-yellow-200 bg-yellow-100 outline-yellow-500 text-black text-end rounded-md" />
      </div>
      <button class="w-24 border border-yellow-200 bg-yellow-100 outline-yellow-500 rounded-md self-center">
        Enviar
      </button>
    </form>
    <div>
      <p v-if="variablesVacias">Favor de rellenar los valores solicitados</p>
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
const constante = ref<number>();
const constanteC = ref<number>();
const modulo = ref<number>();
const variablesVacias = ref<boolean>();
const mensajeAux = ref<string>();

let semillaCheck: HTMLInputElement;
let constanteCheck: HTMLInputElement;
let constanteCCheck: HTMLInputElement;
let moduloCheck: HTMLInputElement;

// Metodos
const formSubmit = (e: any) => {
  e.preventDefault();
  semilla.value = Number(semillaCheck.value);
  constante.value = Number(constanteCheck.value);
  constanteC.value = Number(constanteCCheck.value);
  modulo.value = Number(moduloCheck.value);

  if (semilla.value != 0 && constante.value != 0 && constanteC.value != 0 && modulo.value != 0) {
    // Resetear los checks y añadir border-yellow-200 bg-yellow-100
    semillaCheck.classList.remove("border-red-500", "bg-red-100", "outline-red-500");
    constanteCheck.classList.remove("border-red-500", "bg-red-100", "outline-red-500");
    constanteCCheck.classList.remove("border-red-500", "bg-red-100", "outline-red-500");
    moduloCheck.classList.remove("border-red-500", "bg-red-100", "outline-red-500");

    semillaCheck.classList.add("border-yellow-200", "bg-yellow-100", "outline-yellow-500");
    constanteCheck.classList.add("border-yellow-200", "bg-yellow-100", "outline-yellow-500");
    constanteCCheck.classList.add("border-yellow-200", "bg-yellow-100", "outline-yellow-500");
    moduloCheck.classList.add("border-yellow-200", "bg-yellow-100", "outline-yellow-500");

    variablesVacias.value = false;

    // Logica para hacer el algoritmo lineal
    /*
          i = 1
    while i <= 25:
        res = (a * x0 + c) % m
        numPseudo = res / m
        print(f"Iteracion no.{i}: {res}, Pseudoaleatorio: {numPseudo}\n")
        i += 1
        x0 = res  # Actualizar x0 para el siguiente número
    */

    let i = 1;
    let res = 0;
    let numPseudo = 0;
    mensajeAux.value += 'Iteraciones y pseudoaleatorios'
    while (i <= 150 && semilla.value != 0) {
      res = (constante.value * semilla.value + constanteC.value) % modulo.value;
      numPseudo = res / modulo.value;
      mensajeAux.value += `\nIteracion no.${i}: ${res}, Pseudoaleatorio: ${numPseudo}`;
      i += 1;
      semilla.value = res;
    }
  } else {
    // Resetear los checks y añadir border-red-500 bg-red-100
    semillaCheck.classList.remove("border-yellow-200", "bg-yellow-100", "outline-yellow-500");
    constanteCheck.classList.remove("border-yellow-200", "bg-yellow-100", "outline-yellow-500");
    constanteCCheck.classList.remove("border-yellow-200", "bg-yellow-100", "outline-yellow-500");
    moduloCheck.classList.remove("border-yellow-200", "bg-yellow-100", "outline-yellow-500");

    semillaCheck.classList.add("border-red-500", "bg-red-100", "outline-red-500");
    constanteCheck.classList.add("border-red-500", "bg-red-100", "outline-red-500");
    constanteCCheck.classList.add("border-red-500", "bg-red-100", "outline-red-500");
    moduloCheck.classList.add("border-red-500", "bg-red-100", "outline-red-500");
  }
};

onMounted(() => {
  variablesVacias.value = true;
  mensajeAux.value = "";

  semillaCheck = document.getElementById("semilla") as HTMLInputElement;
  constanteCheck = document.getElementById("constante") as HTMLInputElement;
  constanteCCheck = document.getElementById("constanteC") as HTMLInputElement;
  moduloCheck = document.getElementById("modulo") as HTMLInputElement;

  semillaCheck.classList.add("border-yellow-200", "bg-yellow-100");
  constanteCheck.classList.add("border-yellow-200", "bg-yellow-100");
  constanteCCheck.classList.add("border-yellow-200", "bg-yellow-100");
  moduloCheck.classList.add("border-yellow-200", "bg-yellow-100");

  semillaCheck.focus();
});
</script>
