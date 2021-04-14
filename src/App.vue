<template>
  <div id="app" style="display:flex;">
    <el-form label-width="auto" style="flex-grow:2;">
      <el-form-item label="Angle">
        <el-slider v-model="angle" :min="0.001" :max="0.524" :step="0.001" show-input/>
      </el-form-item>
      <el-form-item label="Épaisseur">
        <el-slider v-model="lineWidth" :min="0.01" :max="2" :step="0.001" show-input/>
      </el-form-item>
      <el-form-item label="Répétitions circulaires">
        <el-slider v-model="circularDivider" :min="1" :max="32" :step="1" show-input/>
      </el-form-item>
      <el-form-item label="Couleur">
        <el-color-picker v-model="color" @active-change="(v) => color = v"></el-color-picker>
      </el-form-item>
      <el-form-item label="Largeur">
        <el-input v-model="width"></el-input>
      </el-form-item>
      <el-form-item label="Hauteur">
        <el-input v-model="height"></el-input>
      </el-form-item>
      <el-form-item label="Statistiques">
        <div>
          Nombre de lignes: {{lineCount}} - Durée du rendu: {{renderDuration}}ms
        </div>
      </el-form-item>
    </el-form>
    <SuperDessin :width="width"
    :height="height"
    :angle="angle"
    :lineWidth="lineWidth"
    :color="color"
    :circularDivider="circularDivider"
    @lineCount="(v) => lineCount = v"
    @renderDuration="(v) => renderDuration = v"
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
      color: '#2866B1',
      width: 990,
      height: 860,
      lineCount: 0,
      renderDuration: 0,
      circularDivider: 6
    }
  },
  mounted: function () {
    console.log(window.innerHeight)
    window.addEventListener('resize', this.onResize);
    this.onResize()
  },
  methods: {
    onResize: function () {
      console.log('onResize')
      this.height = window.innerHeight - 4 // to avoid vertical scrollbar
      this.width = Math.floor(this.height / Math.sqrt(0.75))
    }
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
  margin-top: 0px;
}
body {
  margin-top: 0px;
  margin-bottom: 0px;
}

</style>
