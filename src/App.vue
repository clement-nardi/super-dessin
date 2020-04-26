<template>
  <div id="app" style="display:flex;">
    <div style="flex-grow:2; border:1px groove grey; margin-right: 10px;">
      <div class="param-container">
        <div class="param-label">Angle</div>
        <el-slider class="param-slider" v-model="angle" :min="0.001" :max="0.524" :step="0.001" show-input/>
      </div>
      <div class="param-container">
        <div class="param-label">Épaisseur</div>
        <el-slider class="param-slider" v-model="lineWidth" :min="0.01" :max="2" :step="0.001" show-input/>
      </div>
      <div class="param-container">
        <div class="param-label">Couleur</div>
        <el-color-picker v-model="color" @active-change="(v) => color = v"></el-color-picker>
      </div>
    </div>
    <SuperDessin :width="800"
    :height="800"
    :angle="angle"
    :lineWidth="lineWidth"
    :color="color"
    />
  </div>
</template>

<script>
import SuperDessin from './components/SuperDessin.vue'

export default {
  name: 'App',
  components: {
    SuperDessin
  },
  data: function () {
    return {
      angle: 0.011,
      lineWidth: 0.11,
      color: '#2866B1'
    }
  },
  mounted: function () {
  },
  methods: {
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 10px;
}
.param-container {
  display: flex;
  align-items: center;
}
.param-slider {
  flex-grow: 2;
  padding-right: 10px;
}
.param-label {
  padding: 12px;
  flex-basis: 120px;
}
</style>
