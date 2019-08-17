<template>
  <div id="app">
    <HelloWorld :list="list" :currentID="currentID" @selectItem="selectItem" @reset="reset" />
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'app',
  components: {
    HelloWorld
  },
  data () {
    return {
      currentID: 0
    }
  },
  created () {
    let list = [];
    for (let i = 0; i < 10; i++) {
      list.push({
        id: i+1,
        name: `选项${i}`
      })
    }
    this.list = list;
  },
  methods: {
    selectItem (id) {
      this.currentID = id;
    },
    reset () {
      this.currentID = this.list[0].id;
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
}
ul li {
  list-style: none;
}
</style>
