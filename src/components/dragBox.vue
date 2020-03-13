<template>
    <div class="flexBox" id='moveDiv' @mousedown="down" @touchstart="down" @mousemove="move" @touchmove.prevent="move" @mouseup="end" @touchend="end" @click='handleClick'>
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
            flags: false,
            position: { x: 0, y: 0 },
            nx: '',
            ny: '',
            dx: '',
            dy: '',
            xPum: '',
            yPum: '',
        }
    },
    created() {},
    method: {
        down() {
            let moveDiv = document.getElementById("moveDiv")
            this.flags = true;
            var touch;
            if (event.touches) {
                touch = event.touches[0];
            } else {
                touch = event;
            }
            this.position.x = touch.clientX;
            this.position.y = touch.clientY;
            this.dx = moveDiv.offsetLeft;
            this.dy = moveDiv.offsetTop;
        },
        move() {
            let moveDiv = document.getElementById("moveDiv")
            if (this.flags) {
                var touch;
                if (event.touches) {
                    touch = event.touches[0];
                } else {
                    touch = event;
                }
                this.nx = touch.clientX - this.position.x;
                this.ny = touch.clientY - this.position.y;
                this.xPum = this.dx + this.nx;
                this.yPum = this.dy + this.ny;
                moveDiv.style.left = this.xPum + "px";
                moveDiv.style.top = this.yPum + "px";
                //阻止页面的滑动默认事件；如果碰到滑动问题，1.2 请注意是否获取到 touchmove
                document.addEventListener("touchmove", function() {
                    event.preventDefault();
                }, false);
            }
        },
        //鼠标释放时候的函数
        end() {
            this.flags = false;
        },
        handleClick(){
          this.$emit('on-click')
        }
    }
}
</script>
<style scoped>
.flexBox {
    font-size: 40px;
    cursor: pointer;
    position: absolute;
    z-index: 10000;
    right: 10%;
    bottom: 5%;
}
</style>