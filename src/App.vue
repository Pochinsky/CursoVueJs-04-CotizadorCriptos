<script setup>
// librerias externas
import { ref, reactive } from "vue";
// componentes
import Alerta from "./components/Alerta.vue";
import Spinner from "./components/Spinner.vue";
import Cotizacion from "./components/Cotizacion.vue";
// componsables
import useCripto from "./composables/useCripto";

const {
  monedas,
  criptomonedas,
  cargando,
  cotizacion,
  obtenerCotizacion,
  mostrarResultado,
} = useCripto();

// estado
const cotizar = reactive({
  moneda: "",
  criptomoneda: "",
});
const error = ref("");

// funciones
const cotizarCripto = () => {
  // validacion de campos vacios
  if (Object.values(cotizar).includes("")) {
    error.value = "Todos los campos son obligatorios";
    setTimeout(() => {
      error.value = "";
    }, 3000);
    return;
  }
  obtenerCotizacion(cotizar);
};
</script>

<template>
  <div class="contenedor">
    <h1 class="titulo">Cotizador de <span>Criptomonedas</span></h1>
    <div class="contenido">
      <Alerta v-if="error">{{ error }}</Alerta>
      <form @submit.prevent="cotizarCripto" class="formulario">
        <!-- select monedas -->
        <div class="campo">
          <label for="moneda">Moneda:</label>
          <select name="moneda" id="moneda" v-model="cotizar.moneda">
            <option value="">-- Selecciona --</option>
            <option v-for="moneda in monedas" :value="moneda.codigo">
              {{ moneda.texto }}
            </option>
          </select>
        </div>
        <!-- select criptomonedas -->
        <div class="campo">
          <label for="cripto">Criptomoneda:</label>
          <select name="cripto" id="cripto" v-model="cotizar.criptomoneda">
            <option value="">-- Selecciona --</option>
            <option
              v-for="criptomoneda in criptomonedas"
              :value="criptomoneda.CoinInfo.Name"
            >
              {{ criptomoneda.CoinInfo.FullName }}
            </option>
          </select>
        </div>
        <input type="submit" value="Cotizar" />
      </form>
      <!-- spinner -->
      <Spinner v-if="cargando" />
      <!-- informacion cotizacion -->
      <Cotizacion v-if="mostrarResultado" :cotizacion="cotizacion" />
    </div>
  </div>
</template>
