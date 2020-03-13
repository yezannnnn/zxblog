<template>
    <div class="flexBox" ref='kongtiao' @mousedown="mouseDownHandleelse($event)" @mouseup="mouseUpHandleelse($event)" @click='handleClick'>
        {{ text }}
    </div>
</template>
<script>
export default {
    name: "dragBox",
    props: {
        text: {
            type: String,
            default: "👾"
        },
    },
    data() {
        return {
            touchFlag: false, //拖拽标识
            moveDataelse: {
                x: null,
                y: null
            }
        }
    },
    created() {},
    methods: {
        mouseDownHandleelse (event) {
          this.touchFlag = false
          this.moveDataelse.x = event.pageX - this.$refs.kongtiao.offsetLeft
          this.moveDataelse.y = event.pageY - this.$refs.kongtiao.offsetTop
          event.currentTarget.style.cursor = 'move'
          window.onmousemove = this.mouseMoveHandleelse
        },
        mouseMoveHandleelse (event) {
          // 触发解锁机制

          let x_w = this.$parent.$refs.winner.offsetLeft
          let y_w = this.$parent.$refs.winner.offsetTop

          let x_c = this.$parent.$refs.computer.offsetLeft
          let y_c = this.$parent.$refs.computer.offsetTop

          this.touchFlag = true
          let moveLeft = event.pageX - this.moveDataelse.x + 'px'
          let moveTop = event.pageY - this.moveDataelse.y + 'px'
          this.$refs.kongtiao.style.left = moveLeft
          this.$refs.kongtiao.style.top = moveTop
          // 拖拽到指定位置哦
          if((Math.abs(event.pageX - this.moveDataelse.x - x_w ) <= 40) && (Math.abs(event.pageY - this.moveDataelse.y - y_w) <= 40)) {
            this.$emit("on-winner", this.text, 1)
            return false
          }
          //
          console.log(Math.abs(event.pageX - this.moveDataelse.x - x_c))
          console.log(Math.abs(event.pageY - this.moveDataelse.y - y_c))
          if((Math.abs(event.pageX - this.moveDataelse.x - x_c ) <= 40) && (Math.abs(event.pageY - this.moveDataelse.y - y_c) <= 40)) {
            this.$emit("on-winner", this.text, 2)
            return false
          } 


        },
        mouseUpHandleelse (event) {
          var self = this
          event.stopPropagation();
          window.onmousemove = null
          event.currentTarget.style.cursor = 'move'
          console.log('鼠标松开了')
          // 鼠标松开后 200 毫秒
          setTimeout(function () {
            self.touchFlag = false
          },200)
          
        },
        handleClick(){
          if(this.touchFlag) {
              return false
          }
          this.$emit('on-click')
          console.log('点击我了')
        }
    }
}
</script>
<style scoped>
.flexBox {
    font-size: 40px;
    cursor: pointer;
    position: fixed;
    z-index: 10000;
    top: 95%;
    left: 90%;
}
</style>