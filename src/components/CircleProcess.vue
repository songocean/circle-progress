<!--
/**
 * @module circle-process
 * @desc 圆环进度条
 * @author oceansong
 * @param {Number} [svgWidth=120] svg的宽度
 * @param {Number} [svgHeight=120] svg的高度
 * @param {Number} [radius=55] circle的半径
 * @param {Number} [circleCx=60] circle的圆心横坐标
 * @param {Number} [circleCy=60] circle的圆心纵坐标
 * @param {Number} [strokeWidth=10] circle的宽度
 * @param {String} [bgStroke='#D8D8D8'] 背景circle的颜色
 * @param {String} [stroke='#0055FF'] 前置circle的颜色
 * @param {Number} [angle=-90] 前置circle的起始位置，0点位置是-90、3点位置是0、6点位置是90，以此类推
 * @param {Boolean} [direction=true] 前置circle值运动的方向，true为顺时针、false为逆时针
 * @param {Number} [max=100] 最大值
 * @param {Number} [value=50] 当前值
 * @example
 * <circle-process
    :svgWidth="svgWidth"
    :svgHeight="svgHeight"
    :radius="radius"
    :circleCx="circleCx"
    :circleCy="circleCy"
    :strokeWidth="strokeWidth"
    :bgStroke="bgStroke"
    :stroke="stroke"
    :angle="angle"
    :direction="direction"
  >
  </circle-process>
 */
-->

<template>
  <div class="circle-process" :style="'width: ' + svgWidth + 'px; height: ' + svgHeight + 'px;'">
    <svg class="svg-wrap" width="100%" height="100%">
      <circle
        id="bgCircle"
        :r="radius"
        :cx="circleCx"
        :cy="circleCy"
        :stroke-width="strokeWidth"
        :stroke="bgStroke"
      >
      </circle>
      <circle
        id="svgCircle"
        :r="radius"
        :cx="circleCx"
        :cy="circleCy"
        :stroke="stroke"
        :stroke-width="strokeWidth"
        :stroke-dasharray="computeDashArray"
        :stroke-dashoffset="dashoffset"
        :transform="computeTransform"
      >
        <!-- <animate attributeName="stroke-dashoffset" attributeType="XML" dur="1s" :from="computeDashArray" :to="computeDashoffset" fill="freeze" /> -->
      </circle>
    </svg>
    <slot class="center" name="center"></slot>
  </div>
</template>
<script>
export default {
  name: 'CircleProcess',
  props: {
    svgWidth: {
      type: Number,
      default: 120
    },
    svgHeight: {
      type: Number,
      default: 120
    },
    radius: {
      type: Number,
      default: 55
    },
    circleCx: {
      type: Number,
      default: 60
    },
    circleCy: {
      type: Number,
      default: 60
    },
    strokeWidth: {
      type: Number,
      default: 10
    },
    bgStroke: {
      type: String,
      default: '#D8D8D8'
    },
    stroke: {
      type: String,
      default: '#0055FF'
    },
    angle: {
      type: Number,
      default: -90
    },
    direction: {
      type: Boolean,
      default: true
    },
    max: {
      type: Number,
      default: 100
    },
    value: {
      type: Number,
      default: 50
    }
  },
  data() {
    return {
      dashoffset: 2 * Math.PI * this.radius
    }
  },
  created: function() {
    var dashArray = 2 * Math.PI * this.radius;
    var dashoffsetEnd = (1 - this.value / this.max) * dashArray;
    var dashoffset = dashArray;
    var that = this;
    if (this.direction) {
      var loop = setInterval(function() {
        dashoffset = dashoffset - Math.ceil(Math.random() * 40);
        that.dashoffset = dashoffset;
        if (dashoffset < dashoffsetEnd) {
          dashoffset = dashoffsetEnd;
          that.dashoffset = dashoffset;
          clearInterval(loop);
        }
      }, 100);
    } else {
      return -(1 - this.value / this.max) * dashArray;
    }
  },
  computed: {
    computeDashArray: function() {
      return 2 * Math.PI * this.radius;
    },
    computeDashoffset: function() {

    },
    computeTransform: function() {
      return 'rotate(' + this.angle + ', ' + this.circleCx + ', ' + this.circleCy + ')';
    }
  }
}

</script>
<style scoped>
  .circle-process {
    position: relative;
  }
  #bgCircle {
    fill: transparent;
  }

  #svgCircle {
    fill: transparent;
    transition: stroke-dashoffset 150ms;
    stroke-linecap: round;
  }
  .svg-wrap {
    position: absolute;
    top: 0;
    left: 0;
  }
  .center {
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }
</style>
