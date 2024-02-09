<template>
    <p class="ball">●</p>
</template>
<script>
export default {
    props: {
        speedy: 0,
        speedx: 0,
    },
    data() {
        return {
            speed_x: 0,
            speed_y: 0,
            ball_left: 234,
            ball_top: 468,
        }
    },
    methods: {
        changer() {
            //重力
            if (this.ball_top < 668) {
                this.speed_y += 0.2
            }
            //右と左方向への摩擦
            if (this.speed_x > 0.2) {
                this.speed_x -= 0.01
            } else if (this.speed_x < -0.2) {
                this.speed_x += 0.01
            } else {
                this.speed_x = 0
            }
            const num_top = this.ball_top + this.speed_y
            const num_left = this.ball_left + this.speed_x
            //特殊処理
            //縦方向の位置変化
            if (num_top < 0) {
                this.ball_top = 0
                this.speed_y = -this.speed_y
            } else if (num_top >= 668) {
                // this.speed_y = 20
                // this.speed_x = Math.random() * 50 - 25
                this.ball_top = 668
                this.ball_bound()
            } else {
                this.ball_top = num_top
            }
            //横方向の位置変化
            if (num_left < -7) {
                this.ball_left = -num_left - 7
                this.speed_x = -this.speed_x + 0.01
            } else if (num_left > 491) {
                this.ball_left = 982 - num_left
                this.speed_x = -this.speed_x - 0.01
            } else {
                this.ball_left = num_left
            }

        },
        ball_bound() {
            if (this.speed_y == 0 && this.speed_x == 0) {
                this.speed_x = Math.random()-0.5 * 30 
                this.speed_y = Math.random() * 30 + 30
            }
            this.speed_y = -this.speed_y * 2 / 3
            if (this.speed_y > -2) {
                this.speed_y = 0
            }
            if (this.speed_x > 0.2) {
                this.speed_x -= 0.2
            } else if (this.speed_x < -0.2) {
                this.speed_x += 0.2
            } else {
                this.speed_x = 0
            }
        },
    },
    mounted() {
        this.speed_x = this.speedx
        this.speed_y = this.speedy
        setInterval(this.changer, 10);
    },
}
</script>
<style>
.ball {
    position: absolute;
    font-size: 32px;
    height: 32px;
    width: 32px;
    line-height: 32px;
    top: v-bind(ball_top + 'px');
    left: v-bind(ball_left + 'px');
    color: red;

}
</style>