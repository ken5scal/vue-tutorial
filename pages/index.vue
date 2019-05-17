<template>
  <h1>
    Authorization Code: {{ results }}
    <v-app id="sampleHoge">
      <div>
        result from backend {{ hoge }}
        <div>
          <v-btn
            color="info"
            href="https://dev-991803.oktapreview.com/login/login.htm?fromURI=/oauth2/v1/authorize/redirect?okta_key=lt3HjBGk7_s7T77HRahUxUSQTe2RXJ-jSaIUle3lV7s"
            >Authorize</v-btn
          >
        </div>
        <input v-model.trim="sampleInputText" type="text" name="inStr" />
        <p>{{ kaibun() }}</p>
        <div>
          <button name="myBtn" @click="changeMsg1()">Click Me</button>
        </div>
        <p>txt from button: {{ msgFromButton }}</p>
        <p v-once>txt from button but once: {{ msgFromButton }}</p>
        <p><a :href="toGoogle">Google Search</a></p>
        <v-flex text-xs-center><img v-bind="imageAttrs"/></v-flex>
        <p :style="{ color: color, textAlign: align, fontSize: size + 'em' }">
          css
        </p>
      </div>
    </v-app>
  </h1>
</template>

<script>
import axios from 'axios'

export default {
  el: '#sampleHoge',
  data: function() {
    return {
      msgFromButton: '"button"',
      sampleInputText: '"Please input here"',
      results: [],
      toGoogle: 'http://google.com',
      imageAttrs: {
        src: '/v.png',
        alt: 'Vuetify.js',
        class: 'mb-5'
      },
      color: 'blue',
      align: 'center',
      size: '3'
    }
  },
  computed: {
    hoge() {
      return this.$store.state.albums
    }
  },
  fetch({ store, params }) {
    return axios.get('http://localhost:8080/').then(res => {
      store.commit('add', res.data)
      console.log(store.state.albums)
    })
  },
  methods: {
    kaibun: function() {
      if (this.sampleInputText.length === 0) {
        return
      }
      const rStr = this.sampleInputText
        .split('')
        .reverse()
        .join('')
      if (this.sampleInputText === rStr) {
        return 'yes it is 回分'
      } else {
        return 'no it is 回分'
      }
    },
    changeMsg1: function() {
      this.msgFromButton = 'こんにちは'
    },
    handleClick: function() {
      let fuga = {
        name: 'kengo suzuki'
      }

      axios
        .get(
          'https://dev-991803.oktapreview.com/oauth2/default/v1/authorize?client_id=0oakuhp8brWUfRhGI0h7&response_type=code&scope=openid%20offline_access&redirect_uri=http%3A%2F%2Flocalhost%3A8080%2Fauthorization%2Fcallback&state=state-9aa6bc0-a2a2-4s57-be1a-d0eaaa2fd9bb6ac02'
        )
        .then(res => {
          this.results = res.data
          console.log(res)
          fuga = 'hello'
          console.log(fuga)
        })
    }
  }
}
</script>
