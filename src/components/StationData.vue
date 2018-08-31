<template>
  <v-container fluid>
    <v-slide-y-transition mode="out-in">
      <v-layout column align-center>
        <blockquote>
          {{ data.lanternId }}

          {{ temperaturs   }}
        </blockquote>
      </v-layout>
    </v-slide-y-transition>
  </v-container>
</template>

<script>
import bus from './../bus.js';
import axios from 'axios';

export default {
  name: 'StationData',
  data() {
    return {
      data: '',
      temperaturs: '',
    }
  },
  created() {
    this.callStationEvent();
  },
  methods: {
    getData() {
      let URL = 'http://sisei-p.unifebe.edu.br/smight/api/listarTemperaturaMedia.php/' + this.data.lanternId;

      axios.get(URL).then((response) => {
        this.temperaturs = response.data.temperatura;
      }).catch((error) => {
        console.error(error);
      });
    },
    callStationEvent() {
      bus.$on('callStationData', ($station) => {
        this.getData($station);

        this.data = $station;
      });   
    }
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
