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

  const layer = new Konva.Layer();
    stage.add(layer);

    // 示例部门树数据
    const departmentTreeData = {
        name: 'CEO',
        children: [
            {
                name: 'CTO',
                children: [
                    { name: 'Engineering Manager' },
                    { name: 'Development Team' },
                ],
            },
            {
                name: 'CFO',
                children: [
                    { name: 'Finance Manager' },
                    { name: 'Accounting Team' },
                ],
            },
        ],
    };

    // 递归函数，绘制部门树
    function drawDepartment(node, x, y) {
        const group = new Konva.Group({
            x,
            y,
        });

        const rect = new Konva.Rect({
            width: 150,
            height: 40,
            fill: 'lightblue',
            cornerRadius: 5,
        });

        const text = new Konva.Text({
            text: node.name,
            fontSize: 14,
            fill: 'black',
            width: 150,
            padding: 10,
            align: 'center',
        });

        group.add(rect, text);
        layer.add(group);

        if (node.children) {
            let startX = x - (node.children.length * 75);
            node.children.forEach((child) => {
                startX += 150;
                const line = new Konva.Line({
                    points: [x, y + 20, startX, y + 60],
                    stroke: 'black',
                });
                layer.add(line);
                drawDepartment(child, startX, y + 60);
            });
        }
    }

    drawDepartment(departmentTreeData, window.innerWidth / 2, 50);

    layer.draggable(true)
    stage.draw();
})
</script>

<template>
  <div ref="konvaContainer" id="konvaContainer" class="w-full h-full bg-[#eee]"></div>
</template>