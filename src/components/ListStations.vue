<template>
  <div>
    <md-list class="md-double-line">
        <md-list-item v-for="station in stations" :key="station.id">

          <div class="md-list-item-text">
            <span>{{ station.localizacao }}</span>
          </div>

        </md-list-item>
    </md-list>
  </div>
</template>

<script>
import axios from 'axios';
import bus from "./../bus.js";

export default {
  name: 'ListStations',
  data() {
    return {
      stations: [],
    }
  },
  created() {
    this.getStations();
  },
  methods: {
    getStations() {
      let URL = 'http://sisei-p.unifebe.edu.br/smight/api/listarEstacoes.php';

      axios.get(URL).then((response) => {
        this.stations = response.data.estacoes
      }).catch((error) => {
        console.error(error);
      });
    }
  }
}
</script>

<style scoped>
.md-list {
    width: 320px;
    max-width: 100%;
    display: inline-block;
    vertical-align: top;
    border: 1px solid rgba(#000, .12);
  }
</style>
