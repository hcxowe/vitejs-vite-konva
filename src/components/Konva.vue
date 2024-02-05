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

  var rect = new Konva.Rect({
    x: 20,
    y: 20,
    width: 200,
    height: 150,
    fill: 'white',
    stroke: 'black',
    strokeWidth: 4,
    draggable: true,
  })

  layer.add(rect)
  stage.add(layer)

  // 层级绘制
  layer.draw()

  setTimeout(()=>{
    var startHeight = 150
    var anim = new Konva.Animation(function(frame) {
      var dist = startHeight + 200 * (frame.timeDiff / 1000);
      rect.height(dist);
      startHeight = dist

      if (dist > 500) {
        anim.stop();
      }
    }, layer);

    anim.start();
  },2000)
  
})
</script>

<template>
  <div ref="konvaContainer" id="konvaContainer" class="w-full h-full bg-[#eee]"></div>
</template>

<style scoped>
</style>
