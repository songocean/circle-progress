<template>
  <div id="app">
    <circle-process
      :svgWidth="circleProcess.svgWidth"
      :svgHeight="circleProcess.svgHeight"
      :radius="circleProcess.radius"
      :circleCx="circleProcess.circleCx"
      :circleCy="circleProcess.circleCy"
      :strokeWidth="circleProcess.strokeWidth"
      :bgStroke="circleProcess.bgStroke"
      :stroke="circleProcess.stroke"
      :angle="circleProcess.angle"
      :direction="circleProcess.direction"
      :max="circleProcess.max"
      :value="circleProcess.value"
    >
      <template slot="center">
        <div class="center">
          <p class="center-title">可用额度</p>
          <p class="center-num">¥ {{circleProcess.value}}</p>
        </div>
      </template>
    </circle-process>
    <div class="">信用卡额度：¥{{circleProcess.max}}</div>
  </div>
</template>

<script>
import CircleProcess from './components/CircleProcess.vue'

export default {
  name: 'app',
  data() {
    return {
      circleProcess: {
        svgWidth: 120,
        svgHeight: 120,
        radius: 55,
        circleCx: 60,
        circleCy: 60,
        strokeWidth: 10,
        bgStroke: '#D8D8D8',
        stroke: '#0055FF',
        angle: -90,
        direction: true,
        max: 50000,
        value: 20000
      }
    }
  },
  components: {
    CircleProcess
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
.circle-process {
    margin: 0 auto;
}
.center .center-title {
    font-size: 12px;
    color: #888888;
    margin: 0;
}
.center .center-num {
    font-size: 14px;
    color: #333333;
    margin: 0;
}
</style>
