<script setup>
import { ref, onMounted } from 'vue'
import Konva from 'konva'

const konvaContainer = ref(null)

let stage = null

onMounted(() => {
  stage = new Konva.Stage({
    container: 'konvaContainer',
    width: konvaContainer.value.offsetWidth,
    height: konvaContainer.value.offsetHeight
  })

  /* 监控原生大小变更
  let resizeObserver = new ResizeObserver(e => {
    const { width, height } = e[0].contentRect
    stage.width(width)
    stage.height(height)
  })

  resizeObserver.observe(konvaContainer.value)
  */

  // 创建一个层级 Layer
  var layer = new Konva.Layer()

  // 创建 Shape
  var circle = new Konva.Circle({
    x: stage.width() / 2,
    y: stage.height() / 2,
    radius: 70,
    fill: 'red',
    stroke: 'black',
    strokeWidth: 4,
    draggable: true,
  })

  var rect = new Konva.Rect({
    x: 20,
    y: 20,
    width: 100,
    height: 50,
    fill: 'green',
    stroke: 'black',
    strokeWidth: 4,
    draggable: true,
  })

  // layer加入circle
  layer.add(circle)
  layer.add(rect)

  // layer加入stage
  stage.add(layer)

  // 层级绘制
  layer.draw()
})
</script>

<template>
  <div ref="konvaContainer" id="konvaContainer" class="w-full h-full bg-[#eee]"></div>
</template>

<style scoped>
</style>
