<template>
  <div>
    <canvas
      id="canvas"
      ref="canvas"
      :width="width"
      :height="height"
      class="border border-slate-800"
      @mousedown="startPosition"
      @mouseup="finishedPosition"
      @mouseleave="finishedPosition"
      @mousemove="draw"
      @touchstart="startPosition"
      @touchend="finishedPosition"
      @touchcancel="finishedPosition"
      @touchmove="draw"
    ></canvas>

    <img width="250" height="150" class="border border-slate-800" />

    <div class="btn-group">
      <button class="clear" @click="clear">Clear</button>
      <button class="save">Save</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'SignBoard',
  props: {
    width: {
      type: Number,
      required: true,
    },
    height: {
      type: Number,
      required: true,
    },
  },
  data() {
    return {
      ctx: null,
      isPainting: false,
    }
  },
  mounted() {
    this.initCanvas()
  },
  methods: {
    initCanvas() {
      this.$nextTick(() => {
        this.ctx = this.$refs.canvas.getContext('2d')
        // 設定線條的相關數值
        this.ctx.lineWidth = 4
        this.ctx.lineCap = 'round'
      })
    },
    getPaintPosition(e) {
      const canvas = this.$refs.canvas
      const canvasSize = canvas.getBoundingClientRect()

      if (e.type === 'mousemove') {
        return {
          x: e.clientX - canvasSize.left,
          y: e.clientY - canvasSize.top,
        }
      } else {
        return {
          x: e.touches[0].clientX - canvasSize.left,
          y: e.touches[0].clientY - canvasSize.top,
        }
      }
    },
    startPosition(e) {
      e.preventDefault()
      console.log(e)
      this.isPainting = true
      // this.ctx.beginPath()
    },
    finishedPosition(e) {
      e.preventDefault()
      this.isPainting = false
      // this.ctx.closePath()
      this.ctx.beginPath()
    },
    draw(e) {
      // 滑鼠移動過程中，若非繪圖狀態，則跳出
      if (!this.isPainting) return

      // 取得滑鼠 / 手指在畫布上的 x, y 軸位置位置
      const paintPosition = this.getPaintPosition(e)

      // 移動滑鼠位置並產生圖案
      this.ctx.lineTo(paintPosition.x, paintPosition.y)
      this.ctx.stroke()
    },
    clear() {
      const canvas = this.$refs.canvas
      this.ctx.clearRect(0, 0, canvas.width, canvas.height)
    },
  },
}
</script>

<style lang="scss" scoped></style>
