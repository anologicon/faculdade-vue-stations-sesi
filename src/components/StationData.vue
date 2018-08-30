<template>
  <v-container fluid>
    <v-slide-y-transition mode="out-in">
      <v-layout column align-center>
        <blockquote>
          {{ data.lanternId }}
        </blockquote>
      </v-layout>
    </v-slide-y-transition>
  </v-container>
</template>

<script>
import bus from './../bus.js';

export default {
  name: 'StationData',
  data() {
    return {
      data: '',
    }
  },
  created() {
    this.callStationEvent();
  },
  methods: {
    getData() {
      let URL = 'http://sisei-p.unifebe.edu.br/smight/api/listarTemperaturaMedia.php/' + this.data.lanternId;
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
