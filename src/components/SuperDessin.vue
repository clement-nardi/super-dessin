<template>
  <div>
    <canvas id="dessin" />
  </div>
</template>

<script>
import {point,segment,line,vector} from '@flatten-js/core';

export default {
  name: 'SuperDessin',
  props: {
    width: Number,
    height: Number,
    angle: Number,
    lineWidth: Number,
    color: String,
    circularDivider: Number
  },
  data: function () {
    return {
      canvas: null,
      canvasCtx: null,
      stats: {
        countLineTo: 0
      }
    }
  },
  watch: {
    angle: function () { this.drawDessin() },
    lineWidth:  function () { this.drawDessin() },
    color:  function () { this.drawDessin() },
    width:  function () { this.drawDessin() },
    height:  function () { this.drawDessin() },
    circularDivider:  function () { this.drawDessin() }
  },
  methods: {
    randomPoint: function () {
      return point(  Math.floor(Math.random()*this.width),
        Math.floor(Math.random()*this.height))
    },
    printTriangle: function (tr) {
      /*
      var svg = document.getElementById("mySvg")
      svg.innerHTML += segment(tr[0],tr[1]).svg()
      svg.innerHTML += segment(tr[1],tr[2]).svg()
      svg.innerHTML += segment(tr[2],tr[0]).svg()
      */
      this.canvasCtx.beginPath()
      this.canvasCtx.moveTo(tr[0].x, tr[0].y);
      this.canvasCtx.lineTo(tr[1].x, tr[1].y);
      this.canvasCtx.lineTo(tr[2].x, tr[2].y);
      this.canvasCtx.lineTo(tr[0].x, tr[0].y);
      this.canvasCtx.stroke();

      this.stats.countLineTo += 3
    },
    segmentToLine: function (s) {
      return line(s.ps, s.pe)
    },
    fillTriangle: function (tr_, clockwise) {
      let tr = tr_
      if (!clockwise) {
        tr = [tr_[0], tr_[2], tr_[1]]
      }
      let segments = [segment(tr[0],tr[1]), segment(tr[1],tr[2]), segment(tr[2],tr[0])]
      this.canvasCtx.beginPath()
      this.canvasCtx.moveTo(tr[0].x, tr[0].y);
      while (segments[0].length > 1) {
        let angle = (clockwise?1:-1) * this.angle
        let s1r = segments[0].rotate(angle, segments[0].ps)
        let destPoint = this.segmentToLine(s1r).intersect(this.segmentToLine(segments[1]))[0]
        let s = segment(segments[0].ps, destPoint)
        this.canvasCtx.lineTo(destPoint.x, destPoint.y);
        this.stats.countLineTo += 1
        // svg.innerHTML += s.svg()
        segments = [segment(destPoint, segments[1].pe), segments[2], s]
      }
      this.canvasCtx.stroke();
    },
    drawDessin: function () {
      let begin = new Date()
      this.canvas.width = this.width
      this.canvas.height = this.height
      this.canvasCtx.clearRect(0, 0, this.width, this.height)
      this.canvasCtx.lineWidth = this.lineWidth
      this.canvasCtx.strokeStyle = this.color
      this.canvasCtx.lineJoin = 'round'
      this.stats.countLineTo = 0
      let center = point(this.width/2, this.height/2)
      let centerRight = point(this.width*3/4, this.height/2)
      let seg1 = segment(center, centerRight)
      let seg2 = seg1.rotate(2*Math.PI/this.circularDivider, center)

      let triangle = [center, centerRight, seg2.pe]
      for (let i = 0; i < this.circularDivider; i++) {
        this.printTriangle(triangle)
        this.fillTriangle(triangle, i%2)

        let tr2 = triangle.map(p => p.translate(vector(triangle[0], triangle[1])))
        this.printTriangle(tr2)
        this.fillTriangle(tr2, i%2)

        let tr3 = triangle.map(p => p.translate(vector(triangle[0], triangle[2])))
        this.printTriangle(tr3)
        this.fillTriangle(tr3, i%2)

        let tr4 = [tr2[0], tr2[2], tr3[0]]
        this.printTriangle(tr4)
        this.fillTriangle(tr4, (i+1)%2)

        triangle = triangle.map(p => p.rotate(2*Math.PI/this.circularDivider,center))
      }
      let end = new Date()
      this.$emit('lineCount', this.stats.countLineTo)
      this.$emit('renderDuration', end - begin)
    }

  },
  mounted: function () {
      var canvas = document.getElementById('dessin')
      this.canvas = canvas
      var ctx = canvas.getContext("2d");
      this.canvasCtx = ctx
      this.drawDessin()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
