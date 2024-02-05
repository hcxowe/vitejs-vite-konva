<script setup>
import { ref, onMounted } from 'vue'
import Konva from 'konva'

let treeData = [
  { 
    id: '1', 
    text: '节点1', 
    children: [
      { id: '1-1', text: '节点1-1' },
      { id: '1-2', text: '节点1-2' },
      { id: '1-3', text: '节点1-3' },
      { id: '1-4', text: '节点1-4' }
    ]
  },
  { id: '2', text: '节点2' },
  { id: '3', text: '节点3' },
  { id: '4', text: '节点4' },
  { id: '5', text: '节点5',
    children: [
      { id: '5-1', text: '节点5-1' },
      { id: '5-2', text: '节点5-2' },
      { id: '5-3', text: '节点5-3' },
      { id: '5-4', text: '节点5-4' }
    ] 
  }
]

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

  var mainGroup = new Konva.Group({
    x: 20,
    y: 20,
    draggable: true
  })

  var titleGroup = new Konva.Group({
    x: 0,
    y: 0
  })

  var rect = new Konva.Rect({
    x: 0,
    y: 0,
    width: 300,
    height: 30,
    fill: 'white',
    stroke: 'black',
    strokeWidth: 1
  })

  titleGroup.add(rect)

  var moduleTitle = new Konva.Text({
    x: 5,
    y: 10,
    text: '直升机需求文档',
    fontSize: 14,
    fontFamily: '微软雅黑',
    fill: 'black',
  });

  titleGroup.add(moduleTitle)

  mainGroup.add(titleGroup)
  function showChildren(parentGroup, data) {
    let startX = 0
    let startY = 20

    data.forEach((node, index) => {
      let x = startX
      let y = startY + index * 20

      let group = new Konva.Group({
        id: node.id,
        x: x,
        y: y
      })

      let rect = new Konva.Rect({
        x: 0,
        y: 0,
        width: 300,
        height: 20,
        fill: 'white',
        stroke: 'black',
        strokeWidth: 1
      })

      let label = ''
      if (node.children && node.children.length > 0) {
        label = '+' + node.text
      } else {
        label = '   ' + node.text
      }
      
      let text = new Konva.Text({
        x: parentGroup.padLeft + 10,
        y: 6,
        text: label,
        fontSize: 12,
        fontFamily: '微软雅黑',
        fill: 'black',
      });

      group.isOpen = false
      group.padLeft = parentGroup.padLeft + 10
      group.add(rect).add(text).on('click', function(){
        if (!node.children || node.children.length == 0) {
          return
        }

        if (this.isOpen) {
          this.isOpen = false
          hideChildren()
        } else {
          this.isOpen = true
          showChildren(group, node.children)
        }
      })

      parentGroup.add(group)
    })
  }

  function hideChildren() {

  }

  function showRootNode(mainGroup, data) {
    let startX = 0
    let startY = 30

    data.forEach((node, index) => {
      let x = startX
      let y = startY + index * 20

      let group = new Konva.Group({
        id: node.id,
        x: x,
        y: y,
        draggable: true,
        dragBoundFunc: function(pos) {
          // 阻止拖拽
          let postion = mainGroup.position()

          return {
            x: postion.x + x,
            y: postion.y + y
          }
        }
      })

      let rect = new Konva.Rect({
        x: 0,
        y: 0,
        width: 300,
        height: 20,
        fill: 'white',
        stroke: 'black',
        strokeWidth: 1
      })

      let label = ''
      if (node.children && node.children.length > 0) {
        label = '+' + node.text
      } else {
        label = '   ' + node.text
      }
      
      let text = new Konva.Text({
        x: 5,
        y: 6,
        text: label,
        fontSize: 12,
        fontFamily: '微软雅黑',
        fill: 'black',
      });

      group.isOpen = false
      group.padLeft = 5
      group.add(rect).add(text).on('click', function(){
        if (!node.children || node.children.length == 0) {
          return
        }

        if (this.isOpen) {
          this.isOpen = false
          hideChildren()
        } else {
          this.isOpen = true
          showChildren(group, node.children)
        }
      })

      mainGroup.add(group)
    })
  }

  showRootNode(mainGroup, treeData)

  layer.add(mainGroup)
  stage.add(layer)

  // 层级绘制
  layer.draw()

  /*
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
  */
})
</script>

<template>
  <div ref="konvaContainer" id="konvaContainer" class="w-full h-full bg-[#eee]"></div>
</template>

<style scoped>
</style>
