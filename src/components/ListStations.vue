<template>
  <div class="hello">
      
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
        this.stations = response.data;
      }).catch((error) => {
        console.error(error);
      });
    }
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
