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
      axios.get('https://dev-991803.oktapreview.com/oauth2/default/v1/authorize?client_id=0oakuhp8brWUfRhGI0h7&response_type=code&scope=openid%20offline_access&redirect_uri=http%3A%2F%2Flocalhost%3A8080%2Fauthorization%2Fcallback&state=state-9aa6bc0-a2a2-4s57-be1a-d0eaaa2fd9bb6ac02').then(res => {
        this.results = res.data
      })
    }
  }
}
</script>
