<template>
  <div>
    <div style="height: 500px; width: 500px;">
      <div @drag="handleDrag"
           data-id="12"
           @dragend="handleDragEnd"
           @dragstart="handleDragStart"
           draggable="true">
           <svg width="100%" height="100%">
             <rect x="1" y="1" width="20" height="20" style="fill: red"/>
           </svg>
      </div>
    </div>
  </div>
</template>
<script>
import BarChart from '../components/BarChart'
import LineChart from '../components/LineChart'
import PieChart from '../components/PieChart'
import TTable from '../components/TTable'
import T from '@/assets/lib/TLib'

export default {
  name: 'generalPage',
  components: {
    BarChart,
    PieChart,
    TTable
  },
  data () {
    return {
      drag: {
        target: null,
        isDraging: false,
        lastPosition: {x: 0, y: 0},
        gap: {x: 0, y: 0},
        style: {}
      }
    }
  },
  mounted: function () {
  },
  watch: {
  },
  computed: {
  },
  methods: {
    handleDrag(e) {
      if (this.drag.isDraging && e.clientX !== 0) {
        console.log(e.target.offsetLeft)
        let capX = e.clientX - e.target.offsetLeft
        let capY = e.clientY - e.target.offsetTop
        let left = e.target.offsetLeft + capX + this.drag.gap.x + 'px'
        let top = e.target.offsetTop + capY + this.drag.gap.y + 'px'
        this.drag.target.style.left = left
        this.drag.target.style.top = top
      }
    },
    handleDragEnd(e) {
      this.drag.isDraging = false
      this.drag.target.remove()
    },
    handleDragStart(e) {
      this.lastPosition = {
        x: e.clientX,
        y: e.clientY
      }
      this.drag.gap.x = e.target.offsetLeft - e.clientX
      this.drag.gap.y = e.target.offsetTop - e.clientY
      this.drag.isDraging = true
      this.drag.target = this.copyNewDiv(e.target)
      console.log(this.drag.target)
      this.checkDataset(e.target)
    },

    checkDataset(target) {
      console.log(target.dataset)
    },
    copyNewDiv(target) {
      console.log(target)
      console.log(target.clientWidth)
      let div = target.cloneNode()
      div.innerHTML = target.innerHTML
      div.style.position = 'absolute'
      target.parentNode.appendChild(div)
      return div
    }
  }
}
</script>
