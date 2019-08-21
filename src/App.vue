<template>
  <div id="app">
    <olmap :btnName="btnName"/>
    <div>
      <my-select v-model="townIndex" :data-source="towns"></my-select>
      <my-select v-model="areaIndex" :data-source="districts"></my-select>
      <child-1  :btnName="btnName"/>
      <button v-on:click="changeMap(towns[townIndex] + districts[areaIndex])">送出</button>
    </div>
  </div>
</template>

<script>
import olmap from './components/olmap.vue'
import mySelect from './components/my-select.vue'
import data from './assets/county.json'
import Child1 from './components/Child1'
let cities = data;

let towns = () => cities.map(x => x.name);

let districts = function() {
  return cities[this.townIndex].districts.map(x => x.name);
};

let zip = function() {
  return cities[this.townIndex].districts[this.areaIndex].zip;
};

let townIndex = function() {
  this.areaIndex = 0;
};

export default {
  name: 'app',
  data() {
    return {
      townIndex: 0,
      areaIndex: 0,
      btnName: ''
    }
  },
  components: {
    olmap,
    mySelect,
    Child1
  }, computed: {
    towns,
    districts,
    zip,
  },
  watch: {
    townIndex,
  }, methods: {
    changeMap: function(mapcode){
      this.btnName = mapcode
    }
  }
}
</script>

<style>
*{padding:0; margin:0;}
html,body{
  height: 100%;
}
#app {
  height: 750px;
  width: 750px;
}

</style>