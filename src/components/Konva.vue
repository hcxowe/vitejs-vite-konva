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
  { id: '3', text: '节点3', 
    children: [
      { id: '3-1', text: '节点3-1' },
      { id: '3-2', text: '节点3-2',
        children: [
          { id: '3-2-1', text: '节点3-2-1' },
          { id: '3-2-2', text: '节点3-2-2' },
          { id: '3-2-3', text: '节点3-2-3' },
          { id: '3-2-4', text: '节点3-2-4' }
        ]
      },
      { id: '3-3', text: '节点3-3' },
      { id: '3-4', text: '节点3-4' }
    ] 
  },
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

let treeData2 = [
  { 
    id: '21', 
    text: '节点1', 
    children: [
      { id: '21-1', text: '节点1-1' },
      { id: '21-2', text: '节点1-2' },
      { id: '21-3', text: '节点1-3' },
      { id: '21-4', text: '节点1-4' }
    ]
  },
  { id: '22', text: '节点2' },
  { id: '23', text: '节点3', 
    children: [
      { id: '23-1', text: '节点3-1' },
      { id: '23-2', text: '节点3-2',
        children: [
          { id: '23-2-1', text: '节点3-2-1' },
          { id: '23-2-2', text: '节点3-2-2' },
          { id: '23-2-3', text: '节点3-2-3' },
          { id: '23-2-4', text: '节点3-2-4' }
        ]
      },
      { id: '23-3', text: '节点3-3' },
      { id: '23-4', text: '节点3-4' }
    ] 
  },
  { id: '24', text: '节点4' },
  { id: '25', text: '节点5',
    children: [
      { id: '25-1', text: '节点5-1' },
      { id: '25-2', text: '节点5-2' },
      { id: '25-3', text: '节点5-3' },
      { id: '25-4', text: '节点5-4' }
    ] 
  }
]

let relations = [
  { from: '1', to: '22' },
  { from: '1', to: '23' },
  { from: '2', to: '24' },
  { from: '1-1', to: '25-1' },
  { from: '5-3', to: '23-2-2' },
  { from: '25-3', to: '3-3' },
  { from: '23-2-4', to: '1-2' },
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
  let layer = new Konva.Layer()

  // 条目关系线层
  let linelayer = new Konva.Layer()

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
    fill: '#ccc',
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
  mainGroup.level = 0

  var bodyGroup = new Konva.Group({
    x: 0,
    y: 30,
    draggable: true,
    dragBoundFunc: function(pos) {
      // 阻止拖拽
      let postion = mainGroup.position()

      return {
        x: postion.x + 0,
        y: postion.y + 30
      }
    }
  })

  bodyGroup.level = 0


  var mainGroup2 = new Konva.Group({
    x: 620,
    y: 20,
    draggable: true
  })

  var titleGroup2 = new Konva.Group({
    x: 0,
    y: 0
  })

  var rect2 = new Konva.Rect({
    x: 0,
    y: 0,
    width: 300,
    height: 30,
    fill: '#ccc',
    stroke: 'black',
    strokeWidth: 1
  })

  titleGroup2.add(rect2)

  var moduleTitle2 = new Konva.Text({
    x: 5,
    y: 10,
    text: '战斗机需求文档',
    fontSize: 14,
    fontFamily: '微软雅黑',
    fill: 'black',
  });

  titleGroup2.add(moduleTitle2)

  mainGroup2.add(titleGroup2)
  mainGroup2.level = 0

  var bodyGroup2 = new Konva.Group({
    x: 0,
    y: 30,
    draggable: true,
    dragBoundFunc: function(pos) {
      // 阻止拖拽
      let postion = mainGroup2.position()

      return {
        x: postion.x + 0,
        y: postion.y + 30
      }
    }
  })

  bodyGroup2.level = 0

  function showChildren(rootGroup, parentGroup, treeData, data) {
    let size = 0
    let startX = 0
    let startY = parentGroup.level == 0 ? 0 : 20

    data.forEach((node, index) => {
      let x = startX
      let y = startY + index * 20

      let group = new Konva.Group({
        //id: node.id,
        x: x,
        y: y
      })

      let rect = new Konva.Rect({
        id: node.id,
        x: 0,
        y: 0,
        width: 300,
        height: 20,
        fill: 'white',
        stroke: 'black',
        strokeWidth: 1
      })

      if (typeof node.isOpen == 'undefined') {
        node.isOpen = false
      }

      let label = ''
      if (node.children && node.children.length > 0) {
        label = (node.isOpen ? '- ' : '+') + node.text
      } else {
        label = '   ' + node.text
      }
      
      let text = new Konva.Text({
        x: parentGroup.level * 10 + 10,
        y: 6,
        text: label,
        fontSize: 12,
        fontFamily: '微软雅黑',
        fill: 'black',
      });

      group.level = parentGroup.level + 1
      group.add(rect).add(text).on('click', function(evt){
        evt.cancelBubble = true

        if (!node.children || node.children.length == 0) {
          return
        }

        node.isOpen = !node.isOpen

        rootGroup.destroyChildren()
        showChildren(rootGroup, rootGroup, treeData, treeData)
        rootGroup.draw()

        linelayer.destroyChildren()
        showRelations(relations)
        linelayer.draw()
      })

      size += 1
      if (node.isOpen && node.children && node.children.length > 0) {
        let childSize = showChildren(rootGroup, group, treeData, node.children)
        startY += childSize * 20

        size += childSize
      }

      parentGroup.add(group)
    })

    return size
  }

  function hideChildren() {

  }

  showChildren(bodyGroup, bodyGroup, treeData, treeData)
  showChildren(bodyGroup2, bodyGroup2, treeData2, treeData2)

  mainGroup.add(bodyGroup)
  mainGroup2.add(bodyGroup2)

  layer.add(mainGroup)
  layer.add(mainGroup2)

  
  function showRelations(relations) {
    relations.forEach(relation => {
      let fromGroup = stage.findOne('#' + relation.from)
      let toGroup = stage.findOne('#' + relation.to)

      if (!fromGroup || !toGroup) {
        return
      }

      let fromPostion = fromGroup.absolutePosition()
      let toPostion = toGroup.absolutePosition()

      let fx, tx

      if (fromPostion.x > toPostion.x) {
        fx = fromPostion.x
        tx = toPostion.x + 300
      } else {
        fx = fromPostion.x + 300
        tx = toPostion.x 
      }

      let redLine = new Konva.Arrow({
        points: [fx, fromPostion.y + 10, tx, toPostion.y + 10],
        stroke: 'red',
        strokeWidth: 0.3,
        pointerLength: 6,
        pointerWidth: 6,
        fill: 'red',
        stroke: 'red'
      })

      linelayer.add(redLine)
    })
  }

  stage.add(linelayer)
  stage.add(layer)

  // 层级绘制
  
  layer.draw()

  showRelations(relations)

  linelayer.draw()

  mainGroup.on('dragmove', function () {
    linelayer.destroyChildren()
    showRelations(relations)
    linelayer.draw()
  })

  mainGroup2.on('dragmove', function () {
    linelayer.destroyChildren()
    showRelations(relations)
    linelayer.draw()
  })
})
</script>

<template>
  <div ref="konvaContainer" id="konvaContainer" class="w-full h-full bg-[#eee]"></div>
</template>

<style scoped>
</style>
