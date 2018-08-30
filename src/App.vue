<template>
  <v-app>
    <v-navigation-drawer
      persistent
      :mini-variant="miniVariant"
      :clipped="clipped"
      v-model="drawer"
      enable-resize-watcher
      fixed
      app
    >
      <v-list>
        <v-list-tile
          value="true"
          v-for="(item, i) in items"
          :key="i" 
        >
          <v-list-tile-action>
            <v-icon v-html="item.icon"></v-icon>
          </v-list-tile-action>
          <v-list-tile-content>
            <v-list-tile-title v-text="item.title"></v-list-tile-title>
          </v-list-tile-content>
        </v-list-tile>
        <div v-for="station in stations" :key="station.id">
          <Station :val="station"></Station>
        </div>
      </v-list>
    </v-navigation-drawer>
    <v-toolbar
      app
      :clipped-left="clipped"
    >
      <v-btn icon @click.stop="clipped = !clipped">
        <v-icon>web</v-icon>
      </v-btn>
      <v-toolbar-title v-text="title"></v-toolbar-title>
      <v-spacer></v-spacer>
    </v-toolbar>
    <v-content>
      <StationData/>
    </v-content>
    <v-navigation-drawer
      temporary
      :right="right"
      v-model="rightDrawer"
      fixed
      app
    >
      <v-list>
        <v-list-tile @click="right = !right">
          <v-list-tile-action>
            <v-icon>compare_arrows</v-icon>
          </v-list-tile-action>
        </v-list-tile>
      </v-list>
    </v-navigation-drawer>
    <v-footer :fixed="fixed" app>
      <span>&copy; 2017</span>
    </v-footer>
  </v-app>
</template>

<script>


import StationData from './components/StationData'
import Station from './components/Station'
import axios from 'axios';
import bus from "./bus.js";

export default {
  name: 'App',
  components: {
    StationData, Station
  }, 
  created() {
    this.getStations();
  },
  data () {
    return {
      stations: [],
      clipped: false,
      drawer: true,
      fixed: false,
      items: [{
        icon: 'bubble_chart',
        title: 'Stations'
      }],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'Smart City'
    }
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
  },
}
</script>
