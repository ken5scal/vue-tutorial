<template>
  <h1>
    Authorization Code: {{ results }}
    <v-app id="inspire">
      <div>
        <v-btn color="info" @click="handleClick">Authorize</v-btn>
      </div>
    </v-app>
  </h1>
</template>

<script>
import axios from 'axios'

export default {
  data: function() {
    return {
      results: []
    }
  },
  computed: {
    hoge() {
      return this.$store.state.albums
    }
  },
  fetch({ store, params }) {
    return axios.get('http://localhost:8080/').then(res => {
      // store.commit('add', res.data)
      // console.log(store.state.albums)
    })
  },
  methods: {
    handleClick: function() {
      axios.get('http://localhost:8080/').then(res => {
        this.results = res.data
      })
    }
  }
}
</script>
