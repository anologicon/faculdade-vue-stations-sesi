<template>
  <div>
    <v-list-tile >
      <v-list-tile-action>
        <a href="#"  @click="callData()">
          <v-icon>home</v-icon>
        </a>  
          </v-list-tile-action>
      <v-list-tile-title>
        <a href="#" @click="callData()">
          {{ station.localizacao }}
        </a>
      </v-list-tile-title>
    </v-list-tile>
  </div>
</template>

<script>
import axios from 'axios';
import bus from "./../bus.js";

export default {
  name: 'Station',
  props: ['val'],
  data() {
    return {
      station: this.val,
    }
  },
  methods: {
    callData() {
      bus.$emit('callStationData', this.station);
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
