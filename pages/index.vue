<template>
  <h1>
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
  fetch({ store, params }) {
    return axios.get('http://localhost:8080/').then(res => {
      store.commit('add', res.data)
    })
  }
}
</script>
<script>
export default {
  methods: {
    handleClick: function() {
      alert('test')
    }
  }
}
</script>
