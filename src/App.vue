<script setup>
import { ref } from 'vue';

const container = ref(null)
const draggableBox = ref(null)
let isDragging = false
let offsetX
let offsetY
const startDrag = (e) => {
  isDragging  = true
  //1.计算鼠标相对于元素左上角的偏移量
  offsetX = e.clientX - draggableBox.value.getBoundingClientRect().left;//left是元素相对于视口的left
  offsetY = e.clientY - draggableBox.value.getBoundingClientRect().top;
  //ps:需要给document绑定事件，如果给draggableBox绑定事件，当鼠标移动过快时，会超出draggableBox范围，导致事件失效
  document.addEventListener('mousemove', drag)
  document.addEventListener('mouseup', stopDrag )
}

const drag = (e) => {
  if(!isDragging) return
  const containerRect = container.value.getBoundingClientRect();
  //2.计算偏移量
  const newX = e.clientX - containerRect.left - offsetX;//draggableBox相对于container的left - 1中的offsetX,即为draggableBox所需要移动的距离
  const newY = e.clientY - containerRect.top - offsetY;
  const maxX = container.value.offsetWidth - draggableBox.value.offsetWidth;
  const maxY = container.value.offsetHeight - draggableBox.value.offsetHeight;
  //设置不能超过边界
  draggableBox.value.style.left = Math.max(0,Math.min(newX, maxX)) + 'px';
  draggableBox.value.style.top = Math.max(0,Math.min(newY, maxY))  + 'px';
}

const stopDrag = () => {
  //移除mousemove和mouseup事件
  isDragging = false
  document.removeEventListener('mousemove', drag)
  document.removeEventListener('mouseup', stopDrag)
}

</script>

<template>
  <div>拖拽里面的小正方形</div>
  <div class="container" ref="container">
    <div class="inner" @mousedown="startDrag" ref="draggableBox"></div>
  </div>
</template>

<style scoped>
.container {
  width: 300px;
  height: 300px;
  border: 1px solid #000;
  position: relative;
}

.inner {
  width: 50px;
  height: 50px;
  border: 1px solid blue;
  position: absolute;
  left: 0;
  top: 0;
}
</style>
