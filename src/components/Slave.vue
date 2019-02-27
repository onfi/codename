<template>
  <div class="flex">
    <div v-for="people in expect" v-bind:key="people.index" v-bind:class="people[1]">
    </div>
  </div>
</template>

<script>
export default {
  name: 'Slave',
  props: {
    expect: Array
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.flex {
  display: flex;
  flex-wrap: wrap;
}
.flex > div {
  width: 19%;
  height: 10vw;
  border: solid 1px #eee;
}
.flex > .people {
  background-color: gray;
}
.flex > .agent {
  background-color: green;
}
.flex > .killer {
  background-color: black;
}
</style>
