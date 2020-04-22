<template>
  <div>

    <svg id="mySvg" :width="width" :height="height" style="border:1px solid #d3d3d3;"></svg>

  </div>
</template>

<script>
import {point,segment,line,vector} from '@flatten-js/core';

export default {
  name: 'SuperDessin',
  props: {
    width: Number,
    height: Number
  },
  methods: {
    randomPoint: function () {
      return point(  Math.floor(Math.random()*this.width),
        Math.floor(Math.random()*this.height))
    },
    printTriangle: function (tr) {
      var svg = document.getElementById("mySvg")
      svg.innerHTML += segment(tr[0],tr[1]).svg()
      svg.innerHTML += segment(tr[1],tr[2]).svg()
      svg.innerHTML += segment(tr[2],tr[0]).svg()
    },
    segmentToLine: function (s) {
      return line(s.ps, s.pe)
    },
    fillTriangle: function (tr_, clockwise) {
      let tr = tr_
      if (!clockwise) {
        tr = [tr_[0], tr_[2], tr_[1]]
      }
      var svg = document.getElementById("mySvg")
      let segments = [segment(tr[0],tr[1]), segment(tr[1],tr[2]), segment(tr[2],tr[0])]
      while (segments[0].length > 3) {
        let angle = (clockwise?1:-1) * 0.10
        let s1r = segments[0].rotate(angle, segments[0].ps)
        let destPoint = this.segmentToLine(s1r).intersect(this.segmentToLine(segments[1]))[0]
        let s = segment(segments[0].ps, destPoint)
        svg.innerHTML += s.svg()
        segments = [segment(destPoint, segments[1].pe), segments[2], s]
      }
    }
  },
  mounted: function () {

      let center = point(this.width/2, this.height/2)
      let centerRight = point(this.width*3/4, this.height/2)
      let seg1 = segment(center, centerRight)
      let seg2 = seg1.rotate(Math.PI/3, center)

      let triangle = [center, centerRight, seg2.pe]
      for (let i = 0; i < 6; i++) {
        this.printTriangle(triangle)
        this.fillTriangle(triangle, i%2)

        let tr2 = triangle.map(p => p.translate(vector(triangle[0], triangle[1])))
        this.printTriangle(tr2)
        this.fillTriangle(tr2, i%2)

        let tr3 = tr2.map(p => p.rotate(Math.PI/3, tr2[0]))
        this.printTriangle(tr3)
        this.fillTriangle(tr3, (i+1)%2)

        let tr4 = tr2.map(p => p.rotate(-Math.PI/3, tr2[0]))
        this.printTriangle(tr4)
        this.fillTriangle(tr4, (i+1)%2)

        triangle = triangle.map(p => p.rotate(Math.PI/3,center))
      }

  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
