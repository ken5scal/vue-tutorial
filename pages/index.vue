<template>
  <v-app id="sampleHoge">
    <style>
      .left {
        text-align: left;
      }
      .big {
        font-size: 2em;
        color: red;
      }
    </style>
    Authorization Code: {{ results }}
    <div>
      <div>
        <v-btn
          color="info"
          href="https://dev-991803.oktapreview.com/login/login.htm?fromURI=/oauth2/v1/authorize/redirect?okta_key=lt3HjBGk7_s7T77HRahUxUSQTe2RXJ-jSaIUle3lV7s"
          >Authorize</v-btn
        >
      </div>
      <input
        v-model.trim.lazy="texts.sampleInputText"
        type="text"
        name="inStr"
      />
      <p>{{ kaibun() }}</p>
      <button name="myBtn" @click="changeMsg1('clicked', $event)">
        Click Me
      </button>
      <p :class="{ leftAlign: true, big: enhance }">
        txt from button: {{ texts.msgFromButton }}
      </p>
      <p v-if="enhance">txt from button: {{ texts.msgFromButton }}</p>
      <p v-once :class="{ 'leftAlign big': true }">
        txt from button but once: {{ texts.msgFromButton }}
      </p>
      <a :href="toGoogle">Google Search</a>
      <div @click="move">
        <img v-if="enhance" v-bind="imageAttrs" :style="imageStyle" />
      </div>
      <p :style="[myStyle, newStyle]">
        css
      </p>
      <p>
        算出プロパティ（依存するデータが変更されないと実行されない):
        {{ dateComputed }}
      </p>
      <p>メソッド: {{ dateMethod() }}</p>
      <p>西暦: {{ sYear }} -> 平成 {{ hYear }}</p>
      <ul>
        <li v-for="(y, i) in years" :key="i">{{ y - 2000 }}年</li>
      </ul>
      <table class="table">
        <th />
        <th>Adult</th>
        <th>ID</th>
        <th>Name</th>
        <th>Age</th>
        <tr v-for="customer in sortedAge" :key="customer.id">
          <td>
            <input id="adult" v-model="customer.adult" type="checkbox" />
          </td>
          <td>
            <input
              :id="customer.id"
              v-model="texts.adults"
              type="checkbox"
              :value="customer.name"
            />
          </td>
          <td>{{ customer.adult }}</td>
          <td>{{ customer.id }}</td>
          <td>{{ customer.name }}</td>
          <td>{{ customer.age }}</td>
        </tr>
      </table>

      <ul>
        <li v-for="(a, i) in texts.adults" :key="i">{{ a }}</li>
      </ul>
    </div>
  </v-app>
</template>

<script>
import axios from 'axios'

export default {
  el: '#sampleHoge',
  data: function() {
    return {
      texts: {
        msgFromButton: '',
        sampleInputText: '"Please input here"',
        adults: []
      },
      results: [],
      toGoogle: 'http://google.com',
      imageAttrs: {
        src: '/v.png',
        alt: 'Vuetify.js',
        class: 'mb-5'
      },
      imageStyle: {
        position: 'absolute',
        left: '50px',
        top: '100px',
        transition: 'all 0.5s'
      },
      myStyle: {
        color: 'blue',
        align: 'center',
        fontSize: '3em'
      },
      newStyle: {
        backgroundColor: 'yellow',
        'font-size': '1em' // Same as fontSize but in CSS property naming
      },
      enhance: false,
      sYear: 2000,
      years: [5000, 4000, 3000, 2000, 1000],
      customers: [
        { id: 1, name: 'Kengo Suzuki', age: 31, adult: true },
        { id: 2, name: 'Yumiko Suzuki', age: 65, adult: true },
        { id: 3, name: 'Stan Suzuki', age: 65, adult: true },
        { id: 4, name: 'Goro Suzuki', age: 29, adult: false }
      ]
    }
  },
  computed: {
    // 算出プロパティ
    dateComputed: function() {
      return new Date().toLocaleString()
    },
    hYear: {
      get: function() {
        return this.sYear - 1999
      },
      set: function(newValue) {
        this.sYear = newValue + 1988
      }
    },
    sortedAge: function() {
      return this.customers
        .slice()
        .sort(function(a, b) {
          return a.age - b.age
        })
        .filter(function(customer, index, array) {
          return customer.age <= 31
        })
    }
  },
  // fetch({ store, params }) {
  //   return axios.get('http://localhost:8080/').then(res => {
  //     store.commit('add', res.data)
  //     console.log(store.state.albums)
  //   })
  // },
  methods: {
    dateMethod: function() {
      // you can call method from Computed property as well
      console.log('Date Computed: ' + this.dateComputed)
      return new Date().toLocaleString()
    },
    kaibun: function() {
      if (this.texts.sampleInputText.length === 0) {
        return
      }
      const rStr = this.texts.sampleInputText
        .split('')
        .reverse()
        .join('')
      if (this.texts.sampleInputText === rStr) {
        return 'yes it is 回分'
      } else {
        return 'no it is 回分'
      }
    },
    move: function(e) {
      console.log(e)
      this.imageStyle.left = e.clientX - 60 + 'px'
      this.imageStyle.top = e.clientY - 60 + 'px'
    },
    changeMsg1: function(msg, e) {
      this.texts.msgFromButton = msg
      this.enhance = !this.enhance
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
