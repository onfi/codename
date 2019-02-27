<template>
  <div id="app">
    <input type="button" value="はじめる" v-on:click="start" v-if="state=='qr'" />
    <img v-bind:src="'https://chart.apis.google.com/chart?chs=150x150&cht=qr&chl=https://onfi.github.io/codename?' + seed" v-if="state=='qr'" />
    <Master v-bind:expect=expect v-if="state=='master'" />
    <Slave v-bind:expect=expect_r v-if="state=='slave'" />
  </div>
</template>

<script>
import Master from './components/Master.vue'
import Slave from './components/Slave.vue'
class Random {
  constructor(seed = 88675123) {
    this.x = 123456789;
    this.y = 362436069;
    this.z = 521288629;
    this.w = seed;
  }
  
  // XorShift
  next() {
    let t;
 
    t = this.x ^ (this.x << 11);
    this.x = this.y; this.y = this.z; this.z = this.w;
    return this.w = (this.w ^ (this.w >>> 19)) ^ (t ^ (t >>> 8)); 
  }
}

const AGENT = 'agent'
const KILLER = 'killer'
const PEOPLE = 'people'
let peoples = [
  [KILLER, AGENT], 
  [PEOPLE, AGENT],
  [PEOPLE, AGENT],
  [PEOPLE, AGENT],
  [PEOPLE, AGENT],
  [PEOPLE, AGENT],
  [AGENT, AGENT],
  [AGENT, AGENT],
  [AGENT, AGENT],
  [AGENT, PEOPLE],
  [AGENT, PEOPLE],
  [AGENT, PEOPLE],
  [AGENT, PEOPLE],
  [AGENT, PEOPLE],
  [AGENT, KILLER],
  [KILLER, KILLER],
  [PEOPLE, KILLER],
  [KILLER, PEOPLE],
  [PEOPLE, PEOPLE],
  [PEOPLE, PEOPLE],
  [PEOPLE, PEOPLE],
  [PEOPLE, PEOPLE],
  [PEOPLE, PEOPLE],
  [PEOPLE, PEOPLE],
  [PEOPLE, PEOPLE],
]
let expect
let state
let seed
if(location.search != '') {
  seed = parseInt(location.search.replace(/&.+/,'').replace(/\?/,''))
  expect = reset(seed)
  state = location.search.match(/^\?[0-9]+$/) ? 'slave' : 'master'
} else {
  state = 'qr'
  seed = Math.floor(Math.random() * 100000)
  expect = reset(seed)
}
function reset(s) {
  console.log(s)
  let random = new Random(s)
  return peoples.sort(() => random.next())
}
export default {
  name: 'app',
  components: {
    Master,
    Slave
  },

  methods: {
    start: function() {
      this.state = 'master'
    }
  },
  data() {
    return {
      expect: expect,
      expect_r: expect.reverse(),
      state: state,
      seed: seed
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
