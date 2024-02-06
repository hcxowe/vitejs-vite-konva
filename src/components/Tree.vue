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

const directoryTreeData = {
    name: 'C:',
    type: 'folder',
    isOpen: true,
    children: [
        {
            name: 'Program Files',
            type: 'folder',
            isOpen: false,
            children: [
                { name: 'Adobe', type: 'folder' },
                { name: 'Microsoft Office', type: 'folder' },
                // ... more folders and files
            ],
        },
        {
            name: 'Users',
            type: 'folder',
            isOpen: false,
            children: [
                {
                    name: 'User1',
                    type: 'folder',
                    isOpen: false,
                    children: [
                        { name: 'Documents', type: 'folder' },
                        { name: 'Pictures', type: 'folder' },
                        // ... more folders and files
                    ],
                },
                // ... more users
            ],
        },
        {
            name: 'Windows',
            type: 'folder',
            isOpen: false,
            children: [
                // ... more folders and files
            ],
        },
        // ... more folders and files
    ],
};

function drawDirectoryTree(node, x, y, indentLevel) {
    const group = new Konva.Group({
        x: x + indentLevel * 20,
        y,
    });

    const rect = new Konva.Rect({
        width: 150,
        height: 40,
        fill: node.type === 'folder' ? 'lightblue' : 'lightgreen',
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
        let startY = y + 60;

        node.children.forEach((child) => {
            const line = new Konva.Line({
                points: [x + indentLevel * 20 + 75, y + 20, x + (indentLevel + 1) * 20, startY - 20],
                stroke: 'black',
            });
            layer.add(line);

            // Add event listener to toggle isOpen property
            rect.on('click', () => {
                child.isOpen = !child.isOpen;
                layer.destroyChildren();
                drawDirectoryTree(directoryTreeData, 50, 50, 0);
                stage.draw();
            });

            if (child.isOpen) {
                startY += 60;
            }
        });
        // Update y position for subsequent nodes on the same level
        y = startY;
        node.children.forEach((child) => {
            if (child.isOpen) {
                drawDirectoryTree(child, x, y, indentLevel + 1);
                y += 60;
            }
        });
    }
}

drawDirectoryTree(directoryTreeData, 50, 50, 0);

stage.draw();
})
</script>

<template>
  <div ref="konvaContainer" id="konvaContainer" class="w-full h-full bg-[#eee]"></div>
</template>