<template>
  <v-container fluid>
    <v-slide-y-transition mode="out-in">
      <v-layout column align-center>
        <v-flex>
          <blockquote v-if="data && !ajax">
            <bars :modelData="modelChart"/>
          </blockquote>
        </v-flex>
        <blockquote v-show="!data">
          <v-alert
            :value="true"
            color="info"
            icon="info"
            outline
          >
            Selecione uma estação no menu lateral.
          </v-alert>
        </blockquote>
        <v-progress-circular
            :size="100"
            :width="10"
            color="blue"
            indeterminate=""
            v-if="ajax"
          ></v-progress-circular>
      </v-layout>
    </v-slide-y-transition>
  </v-container>
</template>

<script>
import bus from './../bus.js';
import axios from 'axios';
import Bars from './charts/Bars'

export default {
  name: 'StationData',
  data() {
    return {
      data: '',
      temperaturs: '',
      ajax: false,
      modelChart: {},
    }
  },
  created() {
    this.callStationEvent();
  },
  methods: {
    getData(station) {
      this.data = station;

      let URL = 'http://sisei-p.unifebe.edu.br/smight/api/listarTemperaturaMediaDia.php/' + this.data.lanternId;

      this.ajax = true;
      axios.get(URL).then((response) => {
        this.temperaturs = response.data.temperatura;
        
        this.ajax = false;

        this.ordernateDate();
      }).catch((error) => {
        console.error(error);
      });
    },
    callStationEvent() {
      bus.$on('callStationData', ($station) => {
        this.getData($station);
      });   
    },
    ordernateDate() {
      var dia = [];
      var valores = [];

      this.temperaturs.forEach(temperatur => {
        var dataNFor = new Date(temperatur.dia);
        var format = dataNFor.toLocaleString().split(" ");

        dia.push(format[0]);
        valores.push(parseInt(temperatur.valorConvertido, 10));
      });
      
      var chartModel = {
        labels: [],
        datasets: [
          {
            label: "",
            backgroundColor: '#f87979',
            data: [],
          }
        ]
      }
     
      chartModel.labels = dia.reverse();
      chartModel.datasets[0].data = valores;
      chartModel.datasets[0].label = "Temperatura média por dia";
      chartModel.datasets[0].backgroundColor = '#f87979';    
      
      this.modelChart = chartModel;
    }
  },
  components: {
    Bars
  }
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
