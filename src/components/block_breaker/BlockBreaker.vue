<template>
    <div class="set_name">
        <h2 class="text-center">ブロックくずし</h2>
        <h3 class="comment">操作説明</h3>
        <table class="comment">
            <tr>
                <th>←/→</th>
                <td>:</td>
                <td>左/右移動</td>
            </tr>
            <tr>
                <th>※注意</th>
                <td>:</td>
                <td>板の当てる位置によって跳ね返る角度が変わります。<br>ブロックが表示されていない場合、ページをリロード<br>してみてください。</td>
            </tr>
        </table>
        <v-btn @click="restart()">再挑戦</v-btn>
    </div>
    <div>
        <h2 class="text-center">score:{{ score }}point</h2>
        <div class="game_box">
            <Block v-bind:color="block" v-for="block in blocks" />
            <Ball v-bind:ball="ball" />
            <Raket v-bind:block="raket[0]" />
        </div>
    </div>
</template>
<script>
import Ball from './Ball.vue';
import Block from './Block.vue';
import Raket from './Raket.vue';

export default {
    components: { Ball, Block, Raket },
    data() {
        return {
            blocks: [],
            ball: [0, 0, 320, 450],
            raket: [300, 650],
            keycheck: false,
            key: '',
            next_key: '',
            startCheck: true,
            endCheck: false,
            name: '',
            score: 0,
            number: 0,
        }
    },
    mounted() {
        for (let i = 0; i < 80; i++) {
            this.blocks.push(true)
        }
        document.addEventListener('keydown', this.keydown)
        document.addEventListener('keyup', this.keyup)
        setInterval(this.push_key, 10)
        setInterval(this.changer, 20)
        this.start()
    },
    methods: {
        start() {
            this.ball = [0, 0, 320, 450]
            this.blocks = []
            for (let i = 0; i < 80; i++) {
                this.blocks.push(true)
            }
            this.startCheck = false
            this.endCheck = false
            this.ball[1] = -8
        },
        restart() {
            this.start()
        },
        end() {
            this.ball[1] = 0
            this.ball[0] = 0
            this.addData()
        },
        keydown(event) {
            if (this.key == '') {
                this.key = event.key
            } else if (this.key != event.key) {
                this.next_key = event.key
            }
        },
        keyup() {
            this.key = this.next_key
            this.next_key = ''
        },
        push_key() {
            if ((this.key == 'd' || this.key == 'ArrowRight')) {
                this.raket[0] += 8
            }
            if (this.raket[0] >= 540) {
                this.raket[0] = 540
            }
            if ((this.key == 'a' || this.key == 'ArrowLeft')) {
                this.raket[0] -= 8
            }
            if (this.raket[0] < 0) {
                this.raket[0] = 0
            }
        },
        changer() {
            let x = this.ball[2] + this.ball[0]
            let y = this.ball[3] + this.ball[1]
            //壁の当たり判定
            if (x > 625) {
                x = -x + 1250
                this.ball[0] = -this.ball[0]
            }
            if (x < 0) {
                x = -x
                this.ball[0] = -this.ball[0]
            }
            if (y > 690) {
                y = 690
                this.end()
            }
            if (y < 0) {
                y = -y
                this.ball[1] = -this.ball[1]
            }
            //ラケットの当たり判定
            if (
                this.ball[2] > this.raket[0] - 8 &&
                this.ball[2] < this.raket[0] + 108 &&
                this.ball[3] >= this.raket[1] - 18 && this.ball[3] <= this.raket[1]) {
                y = -y + 1260
                const a = this.ball[2] - this.raket[0] - 50
                this.ball[0] = a / 12.5
                this.ball[1] = Math.abs(a / 12.5) - 8
            }
            //ブロックの当たり判定
            let check_end = true
            for (let i = 0; i < 10; i++) {
                for (let l = 0; l < 8; l++) {
                    if (!this.blocks[i * 8 + l]) {
                        continue
                    } else {
                        check_end = false
                    }
                    let block_check = false
                    const block_x = l * 80
                    const block_y = i * 30
                    //各当たり判定
                    const match_x = this.ball[2] >= block_x && this.ball[2] < block_x + 80
                    //上
                    if (
                        this.ball[3] > block_y &&
                        this.ball[3] < block_y + 8 &&
                        match_x &&
                        !this.blocks[(i - 1) * 8 + l]
                    ) {
                        block_check = true
                        y = block_y - 8
                        this.ball[1] = -this.ball[1]
                    }
                    //下
                    if (this.ball[3] > block_y + 22 &&
                        this.ball[3] < block_y + 30 &&
                        match_x &&
                        (i + 1 == 5 ||
                            !this.blocks[(i + 1) * 8 + l])) {
                        block_check = true
                        y = block_y + 38
                        this.ball[1] = -this.ball[1]
                    }
                    const match_y = this.ball[3] >= block_y &&
                        this.ball[3] < block_y + 30
                    //左
                    if (match_y &&
                        this.ball[2] > block_x - 8 &&
                        this.ball[2] < block_x &&
                        !this.blocks[i * 8 + l - 1]) {
                        block_check = true
                        x = block_x - 8
                        this.ball[0] = -Math.abs(this.ball[0])
                    }
                    //右
                    if (match_y &&
                        this.ball[2] > block_x + 72 &&
                        this.ball[2] < block_x + 80 &&
                        !this.blocks[i * 8 + l + 1]) {
                        block_check = true
                        x = block_x + 88
                        this.ball[0] = Math.abs(this.ball[0])
                    }
                    if (block_check) {
                        this.blocks[i * 8 + l] = false
                        this.score += 100
                    }
                }
            }
            if (check_end) {
                this.end()
            }
            this.ball[2] = x
            this.ball[3] = y
        },
        async addData() {
            const data = {
                name: this.name,
                point: this.score
            }
            const param = {
                method: 'POST',
                headers: {
                    "Content-Type": "application/json",
                },
                body: JSON.stringify(data),
            }
            const response = await fetch('http://54.199.63.195/api/blockbreaker/add', param);
            const result = await response.json();
            this.number = result[0]
            this.endCheck = true
        },
    }
}
</script>
<style src="@/styles/form.css"></style>
<style scoped>
h2 {
    color: green;
}

.game_box {
    position: relative;
    width: 647px;
    padding-bottom: 400px;
    margin: 30px auto;
    display: flex;
    align-items: flex-start;
    flex-wrap: wrap;
    border: 4px solid green;
    background-color: white;
}
</style>