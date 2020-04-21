<template>
  <div>

    <svg id="mySvg" :width="width" :height="height" style="border:1px solid #d3d3d3;"></svg>

  </div>
</template>

<script>
import {Point,Circle,Segment} from '@flatten-js/core';

export default {
  name: 'HelloWorld',
  props: {
    width: Number,
    height: Number
  },
  methods: {
    randomPoint: function () {
      return new Point(  Math.floor(Math.random()*this.width),
        Math.floor(Math.random()*this.height))
    },
    printTriangle: function (tr) {
      var svg = document.getElementById("mySvg")
      svg.innerHTML += new Segment(tr[0],tr[1]).svg()
      svg.innerHTML += new Segment(tr[1],tr[2]).svg()
      svg.innerHTML += new Segment(tr[2],tr[0]).svg()
    },
    fillTriangle: function (tr) {
      var svg = document.getElementById("mySvg")
      let s1 = new Segment(tr[0],tr[1])
      let s1r = s1.rotate(0.12, tr[0])
      svg.innerHTML += s1r.svg()
    }
  },
  mounted: function () {
    var svg = document.getElementById("mySvg")

    // make some construction
    let c1 = new Circle(new Point(200, 110), 50)
    let s1 = new Segment(10, 10, 200, 200)

    svg.innerHTML += c1.svg()
    svg.innerHTML += s1.svg()


    for (let i = 0; i < 3; i++) {
      let tr = [this.randomPoint(), this.randomPoint(), this.randomPoint()]
      this.printTriangle(tr)
      this.fillTriangle(tr)
    }


  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
