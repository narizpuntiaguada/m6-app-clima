<template>
  <div id="app">
    <header class="py-3">
      <h1 class="text-center">Probando API clima.</h1>
    </header>

    <main class="container">
      <section class="infoClima">
        <button class="btn btn-info" @click="actualizarClima">
          Actualizar clima
        </button>
        <p>Última consulta: {{ fechaConsulta }}</p>

        <div class="py-3 over" v-if="datosClima.length">
          <table class="table">
            <thead>
              <tr>
                <th scope="col">Código</th>
                <th scope="col">Estación</th>
                <th scope="col">Hora últ. reporte</th>
                <th scope="col">Temp.</th>
                <th scope="col">Húmedad</th>
                <th scope="col">Estado</th>
                <th scope="col">Ícono</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(clima, index) in datosClima" :key="index">
                <td>{{ clima.Codigo }}</td>
                <td>{{ clima.Estacion }}</td>
                <td>{{ clima.HoraUpdate}}</td>
                <td>{{ clima.Temp }}</td>
                <td>{{ clima.Humedad }}</td>
                <td>{{ clima.Estado }}</td>
                <td class="fs-1">{{iconoClima(clima.Icono)}}</td>
              </tr>
            </tbody>
          </table>
        </div>
        <div v-else>
          <h2 class="text-center py-3">No hay información del clima.</h2>
        </div>
      </section>
    </main>
  </div>
</template>

<script>
import axios from "axios";
import moment from 'moment';

export default {
  name: "App",
  components: {},
  data() {
    return {
      datosClima: [],
    };
  },
  methods: {
    actualizarClima: async function () {
      try {
        let response = await axios.get(
          "https://api.gael.cloud/general/public/clima"
        );

        if (response.status == 200) {
          this.datosClima = response.data;
          this.fechaConsulta = moment().format("DD/MM/YYYY hh:mm:ss a");
        } else {
          throw new Error("Problemas al obtener información del clima");
        }
      } catch (error) {
        console.log(error);
        alert("Ha ocurrido un error al actualizar el clima.");
      }
    },
    iconoClima: function(nombreIcono){
     let iconosClima = {
        "despejado.png": "🌤️",
        "parcial.png": "☁️",
        "cubierto.png": "⛅",
        "pocasnubes.png" : "⛅",
        "niebla.png": "🌫️",
        "llovizna.png": "🌧️",
        "parcialalta.png": "🌥️",
        "cubiertohumo.png": "🌫️"
      }
      return iconosClima[nombreIcono];
      

    }
  },
};
</script>

<style>
</style>
