<script setup>
import { ref, onMounted } from 'vue'
import Konva from 'konva'
import { DagreLayout } from '@antv/layout'

let treeData = {
  nodes: [
    {
      id: '1',
      title: '直升机需求文档',
      entryList: [
        { 
          id: '11', 
          text: '节点11', 
          children: [
            { id: '11-1', text: '节点11-1' },
            { id: '11-2', text: '节点11-2' },
            { id: '11-3', text: '节点11-3' },
            { id: '11-4', text: '节点11-4' }
          ]
        },
        { id: '12', text: '节点12' },
        { id: '13', text: '节点13', 
          children: [
            { id: '13-1', text: '节点13-1' },
            { id: '13-2', text: '节点13-2',
              children: [
                { id: '13-2-1', text: '节点13-2-1' },
                { id: '13-2-2', text: '节点13-2-2' },
                { id: '13-2-3', text: '节点13-2-3' },
                { id: '13-2-4', text: '节点13-2-4' }
              ]
            },
            { id: '13-3', text: '节点13-3' },
            { id: '13-4', text: '节点13-4' }
          ] 
        },
        { id: '14', text: '节点4' },
        { id: '15', text: '节点5',
          children: [
            { id: '15-1', text: '节点15-1' },
            { id: '15-2', text: '节点15-2' },
            { id: '15-3', text: '节点15-3' },
            { id: '15-4', text: '节点15-4' }
          ] 
        }
      ]
    },
    {
      id: '2',
      title: '轰炸机需求文档',
      entryList: [
        { 
          id: '21', 
          text: '节点21', 
          children: [
            { id: '21-1', text: '节点21-1' },
            { id: '21-2', text: '节点21-2' },
            { id: '21-3', text: '节点21-3' },
            { id: '21-4', text: '节点21-4' }
          ]
        },
        { id: '22', text: '节点22' },
        { id: '23', text: '节点23', 
          children: [
            { id: '23-1', text: '节点23-1' },
            { id: '23-2', text: '节点23-2',
              children: [
                { id: '23-2-1', text: '节点23-2-1' },
                { id: '23-2-2', text: '节点23-2-2' },
                { id: '23-2-3', text: '节点23-2-3' },
                { id: '23-2-4', text: '节点23-2-4' }
              ]
            },
            { id: '23-3', text: '节点23-3' },
            { id: '23-4', text: '节点23-4' }
          ] 
        },
        { id: '24', text: '节点24' },
        { id: '25', text: '节点25',
          children: [
            { id: '25-1', text: '节点25-1' },
            { id: '25-2', text: '节点25-2' },
            { id: '25-3', text: '节点25-3' },
            { id: '25-4', text: '节点25-4' }
          ] 
        }
      ]
    },
    {
      id: '3',
      title: '运载机需求文档',
      entryList: [
        { 
          id: '31', 
          text: '节点31', 
          children: [
            { id: '31-1', text: '节点31-1' },
            { id: '31-2', text: '节点31-2' },
            { id: '31-3', text: '节点31-3' },
            { id: '31-4', text: '节点31-4' }
          ]
        },
        { id: '32', text: '节点32' },
        { id: '33', text: '节点33', 
          children: [
            { id: '33-1', text: '节点33-1' },
            { id: '33-2', text: '节点33-2',
              children: [
                { id: '33-2-1', text: '节点33-2-1' },
                { id: '33-2-2', text: '节点33-2-2' },
                { id: '33-2-3', text: '节点33-2-3' },
                { id: '33-2-4', text: '节点33-2-4' }
              ]
            },
            { id: '33-3', text: '节点33-3' },
            { id: '33-4', text: '节点33-4' }
          ] 
        },
        { id: '34', text: '节点34' },
        { id: '35', text: '节点35',
          children: [
            { id: '35-1', text: '节点35-1' },
            { id: '35-2', text: '节点35-2' },
            { id: '35-3', text: '节点35-3' },
            { id: '35-4', text: '节点35-4' }
          ] 
        }
      ]
    },
    {
      id: '4',
      title: '隐形机需求文档',
      entryList: [
        { 
          id: '41', 
          text: '节点41', 
          children: [
            { id: '41-1', text: '节点41-1' },
            { id: '41-2', text: '节点41-2' },
            { id: '41-3', text: '节点41-3' },
            { id: '41-4', text: '节点41-4' }
          ]
        },
        { id: '42', text: '节点42' },
        { id: '43', text: '节点43', 
          children: [
            { id: '43-1', text: '节点43-1' },
            { id: '43-2', text: '节点43-2',
              children: [
                { id: '43-2-1', text: '节点43-2-1' },
                { id: '43-2-2', text: '节点43-2-2' },
                { id: '43-2-3', text: '节点43-2-3' },
                { id: '43-2-4', text: '节点43-2-4' }
              ]
            },
            { id: '43-3', text: '节点43-3' },
            { id: '43-4', text: '节点43-4' }
          ] 
        },
        { id: '44', text: '节点44' },
        { id: '45', text: '节点45',
          children: [
            { id: '45-1', text: '节点45-1' },
            { id: '45-2', text: '节点45-2' },
            { id: '45-3', text: '节点45-3' },
            { id: '45-4', text: '节点45-4' }
          ] 
        }
      ]
    },
    {
      id: '5',
      title: '民用机需求文档',
      entryList: [
        { 
          id: '51', 
          text: '节点51', 
          children: [
            { id: '51-1', text: '节点51-1' },
            { id: '51-2', text: '节点51-2' },
            { id: '51-3', text: '节点51-3' },
            { id: '51-4', text: '节点51-4' }
          ]
        },
        { id: '52', text: '节点52' },
        { id: '53', text: '节点53', 
          children: [
            { id: '53-1', text: '节点53-1' },
            { id: '53-2', text: '节点53-2',
              children: [
                { id: '53-2-1', text: '节点53-2-1' },
                { id: '53-2-2', text: '节点53-2-2' },
                { id: '53-2-3', text: '节点53-2-3' },
                { id: '53-2-4', text: '节点53-2-4' }
              ]
            },
            { id: '53-3', text: '节点53-3' },
            { id: '53-4', text: '节点53-4' }
          ] 
        },
        { id: '54', text: '节点54' },
        { id: '55', text: '节点55',
          children: [
            { id: '55-1', text: '节点55-1' },
            { id: '55-2', text: '节点55-2' },
            { id: '55-3', text: '节点55-3' },
            { id: '55-4', text: '节点55-4' }
          ] 
        }
      ]
    },
  ],
  edges: [
    { source: '1', target: '2' },
    { source: '2', target: '3' },
    { source: '2', target: '4' },
    { source: '3', target: '5' },
    { source: '4', target: '5' }
  ]
}

let relations = [
  { from: '11', to: '22' }
]

const dagreLayout = new DagreLayout({
  begin: [0, 0],
  type: 'dagre',
  rankdir: 'RL',
  ranksep: 150,
  nodesepFunc: (d) => {
    return d.entryList.length * 20 + 30
  }
})

dagreLayout.layout(treeData)

const konvaContainer = ref(null)
let stage = null

onMounted(() => {
  stage = new Konva.Stage({
    container: 'konvaContainer',
    width: konvaContainer.value.offsetWidth,
    height: konvaContainer.value.offsetHeight
  })

  // 创建一个层级 Layer
  let layer = new Konva.Layer()

  // 条目关系线层
  let linelayer = new Konva.Layer()

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

  function createTree(node) {
    var mainGroup = new Konva.Group({
      x: node.x,
      y: node.y,
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
      text: node.title,
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

    showChildren(bodyGroup, bodyGroup, node.entryList, node.entryList)

    mainGroup.add(bodyGroup)
    layer.add(mainGroup)
  }

  console.log(dagreLayout.nodes)

  dagreLayout.nodes.forEach(node => {
    createTree(node)
  })
  
  stage.add(layer)
  stage.add(linelayer)

  layer.draw()
  
  showRelations(relations)
  linelayer.draw()
})
</script>

<template>
  <div ref="konvaContainer" id="konvaContainer" class="w-full h-full bg-[#eee]"></div>
</template>
  
<style scoped>
</style>