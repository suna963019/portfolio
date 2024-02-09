<template>
    <div>

    </div>
</template>
<script>
export default {
    methods: {
        slop_left(element) {
            var c = element;
            var ctx = c.getContext('2d');
            var w = c.clientWidth;
            var h = c.clientHeight;


            c.width = w;
            c.height = h;

            ctx.beginPath();
            ctx.moveTo(w, h);
            ctx.lineTo(0, 0);
            ctx.lineTo(0, h);
            ctx.closePath();
            ctx.fill();
            ctx.fillStyle = "#d841ba";
        },
        slop_right() {
            var c = document.getElementById('canvas02');
            var ctx = c.getContext('2d');
            var w = c.clientWidth;
            var h = c.clientHeight;


            c.width = w;
            c.height = h;

            ctx.beginPath();
            ctx.moveTo(w, h);
            ctx.lineTo(0, h);
            ctx.lineTo(w, 0);
            ctx.closePath();
            ctx.fill();
            ctx.fillStyle = "#d841ba";
        },
        shield() {
            //直線
            const a = [1, 3]
            const b = [4, 6]
            //点
            const c = [2, 5]
            const x = a[1] - b[1] / a[0] - b[0]
            const Px = -1 / x
            
        }
    },
    mounted() {
        this.block_left()
        this.block_right()
    }
}
</script>
<style>
canvas {
    position: absolute;
}

#canvas01 {
    left: 0;
    bottom: 0;
    width: 200px;
    height: 200px;
}

#canvas02 {
    right: 0;
    bottom: 0;
    width: 200px;
    height: 200px;
}
</style>