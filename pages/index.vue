<template>
  <v-app id="sampleHoge">
    <style>
      .box {
        width: 100px;
        height: 100px;
        background: blue;
        <!-- they are defined in Vue attr -->
        <!-- left: 500px;
        right: 500px; -->
        <!-- transition: all 1s ease-in; -->
        <!-- this can be defined in data of Vue -->
      }
      @keyframes myAnime {
        0% {
          transform: rotate(0deg) scale(1) translateX(-200px);
          background: yellow;
        }
        50% {
          transform: rotate(0deg) scale(1) translateX(-100px);
          background: green;
        }
        100% {
          transform: translateX(0px) rotate(360deg);
        }
      }
      .box-enter {
        translateX(-200px)
        opacity: 0;
      }
      .box-enter-active {
        animation: myAnime 3s ease-in;
      }
      .box-leave-to {
        transform: translateX(-200px) scale(0.1) rotate(360deg);
        opacity: 1;
      }
      .toDoList-enter {
        opacity: 0;
      }
      .toDoList-enter-active {
        transform: translateX(200px);
        position: absolute;
        transition: all 1s;
      }
      .toDoList-leave-active {
        position: absolute;
        opacity: 1;
        transition: all 1s;
      }
      .toDoList-leave-to {
        transform: translateX(200px);
        opacity: 0;
      }
      .toDoList-move {
        transition: transform 1s;
      }
      .v-enter, .v-leave-to {
        transform: translateX(100px);
        opacity: 0;
      }
      .v-enter-active, .v-leave-active, .box-leave-active {
        transition: all 1s ease-in;
      }
      .left {
        text-align: left;
      }
      .big {
        font-size: 2em;
        color: red;
      }
      .delbtn {
        margin-left: 10px;
      }
      .done {
        color: gray;
        text-decoration: line-through;
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
        <transition name="box">
          <img v-if="enhance" v-bind="boxImageAttrs" :style="boxImageStyle" />
        </transition>
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
      <input v-model.trim="newToDo" type="text" placeholder="put new to do" />
      <button @click="addTodo">Add Todo</button>
      <transition-group tag="ul" name="toDoList">
        <li
          v-for="(t, i) in toDos"
          :key="t.text"
          :class="{ done: t.done }"
          @mouseover="t.hover = true"
          @mouseout="t.hover = false"
        >
          <input v-model="t.done" type="checkbox" />
          <span>{{ t.text }}</span>
          <v-btn v-show="t.done && t.hover" color="warn" @click="removeToDo(i)"
            >x</v-btn
          >
        </li>
      </transition-group>
      <p>Today is {{ date | jaDay | addJaYobi('だよ') }}</p>
    </div>
  </v-app>
</template>

<script>
import axios from 'axios'

export default {
  el: '#sampleHoge',
  filters: {
    // Filter is an option
    // Can be defined globally
    // Cannot refer to `this` contexts
    jaDay: function(date) {
      const days = ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'fri', 'Sat']
      return days[date.getDay()]
    },
    addJaYobi: function(date, end) {
      if (end) {
        return date + '曜日' + end
      } else {
        return date + '曜日'
      }
    }
  },
  data: function() {
    return {
      texts: {
        msgFromButton: '',
        sampleInputText: '"Please input here"',
        adults: []
      },
      results: [],
      toGoogle: 'http://google.com',
      boxImageAttrs: {
        src: '/v.png',
        alt: 'Vuetify.js',
        class: 'box' // this class is defined on <style>
      },
      boxImageStyle: {
        // this can be defined in <style>
        position: 'absolute',
        left: '400px',
        top: '100px'
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
      ],
      newToDo: '',
      toDos: [],
      localStorageKey: 'toDoList',
      date: new Date()
    }
  },
  computed: {
    // 算出プロパティ
    // It's a property. Methods is a method
    // You can add a getter and setter
    // It will be cached.
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
  watch: {
    toDos: {
      handler: function() {
        localStorage.setItem(this.localStorageKey, JSON.stringify(this.toDos))
      },
      deep: true
    }
  },
  mounted: function() {
    const dataStr = localStorage.getItem(this.localStorageKey)
    if (dataStr) {
      this.toDos = JSON.parse(dataStr)
    }
  },
  methods: {
    // Method Options
    // Can depend on `this` context
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
    },
    addTodo: function() {
      if (this.newToDo === '') {
        return
      }
      this.toDos.push({ text: this.newToDo, done: false, hover: false })
      this.newToDo = ''
    },
    removeToDo: function(idx) {
      if (this.toDos[idx].done === true) {
        this.toDos.splice([idx], 1)
      }
    }
  }
}
</script>
