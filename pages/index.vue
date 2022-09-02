<template>
  <div>
    <v-autocomplete
      v-model="prefecture"
      :items="prefectures"
      label="県"
      dense
      outlined
      @change="getLines"
    ></v-autocomplete>
    <v-autocomplete
      v-model="line"
      :items="lines"
      label="路線"
      dense
      outlined
      @change="getStations"
    ></v-autocomplete>
    <v-autocomplete
      v-model="station"
      :items="stations"
      item-text="name"
      :item-value="(item) => item.x + ',' + item.y"
      label="駅"
      dense
      outlined
      @change="gotoSaunaIkitai"
    ></v-autocomplete>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      prefectures: [],
      prefecture: '',
      lines: [],
      line: '',
      stations: [],
      station: '',
    }
  },
  mounted() {
    axios
      .get('http://express.heartrails.com/api/json?method=getPrefectures')
      .then(
        (response) => (this.prefectures = response.data.response.prefecture)
      )
  },
  methods: {
    getLines() {
      axios
        .get(
          'http://express.heartrails.com/api/json?method=getLines&prefecture=' +
            this.prefecture
        )
        .then((response) => (this.lines = response.data.response.line))
    },
    getStations() {
      axios
        .get(
          'https://express.heartrails.com/api/json?method=getStations&line=' +
            this.line +
            '&prefecture=' +
            this.prefecture
        )
        .then((response) => (this.stations = response.data.response.station))
    },
    gotoSaunaIkitai() {
      window.open(
        'https://sauna-ikitai.com/search?dist=inf&point=' + this.station,
        '_blank'
      )
    },
  },
}
</script>