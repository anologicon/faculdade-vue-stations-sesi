<template>
  <v-container fluid>
    <v-slide-y-transition mode="out-in">
      <v-layout column align-center>
        <blockquote v-if="data && !ajax">
         <bars :modelData="modelChart"/> 
        </blockquote>
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

      let URL = 'http://sisei-p.unifebe.edu.br/smight/api/listarTemperaturaMedia.php/' + this.data.lanternId;

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

      this.temperaturs.forEach(temperatur => {
        if (temperatur) {
          if(dia[temperatur.dia] == null) {
            dia[temperatur.dia] = {valor: 0, quantidade: 0};
          }

          dia[temperatur.dia].valor += parseInt(temperatur.valorConvertido, 10);
          dia[temperatur.dia].quantidade++;
        }
        
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

      var label = [];
      var valores = [];
      
      for (var key in dia) {
        dia[key].valor = Math.round(dia[key].valor / dia[key].quantidade); 
        
        label.push(key);
        valores.push(dia[key].valor);
      }
      
      chartModel.labels = label;
      chartModel.datasets.data = valores;
      chartModel.datasets.label = "Temperatura média por dia";
      chartModel.datasets.backgroundColor = '#f87979';    
      
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
