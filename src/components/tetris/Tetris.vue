<template>
    <div class="set_name">
        <h2 class="text-center">テトリス</h2>
        <h3 class="comment">操作説明</h3>
        <table class="comment">
            <tr>
                <th>矢印キー上（↑）</th>
                <td>:</td>
                <td>ブロックが右回転</td>
            </tr>
            <tr>
                <th>矢印キー右（→）</th>
                <td>:</td>
                <td>ブロックが右移動</td>
            </tr>
            <tr>
                <th>矢印キー上（←）</th>
                <td>:</td>
                <td>ブロックが左移動</td>
            </tr>
            <tr>
                <th>矢印キー下（↓）</th>
                <td>:</td>
                <td>ブロックが最下段に落ちる</td>
            </tr>
            <tr>
                <th>スペースキー</th>
                <td>:</td>
                <td>ブロックのキープと取り出し（一度目はキープのみ）</td>
            </tr>
        </table>
        <v-btn @click="restart_game()">再挑戦</v-btn>
    </div>
    <div class="game">
        <div class="d-flex">
            <div id="stage">
                <div v-for="stage_lane in stage">
                    <Block v-for="stage_block in stage_lane" v-bind:block="stage_block" />
                </div>
                <div id="fall_block">
                    <Blocks v-bind:blocks="nowBlocks" />
                </div>
                <div id="outline"></div>
            </div>
            <div>
                <h2 class="box_title">next</h2>
                <div id="next">
                    <div class="relative">
                        <Block v-for="nextBlock in nextBlocks" v-bind:block="nextBlock" />
                    </div>
                </div>
                <h2 class="box_title">keep</h2>
                <div id="keep">
                    <div class="relative">
                        <Block v-for="keepBlock in keepBlocks" v-bind:block="keepBlock" />
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
import Block from './Block.vue';
import Blocks from './Blocks.vue';

export default {
    components: {
        Block,
        Blocks,
    },
    data() {
        return {
            //各ブロックは[x座標,y座標,色]で表す
            stage: [],
            //天井＋２まで増殖
            //０番目は最下段一個下のストッパー
            defaultBlocks: [
                [[0, 0, 1], [0, 1, 1], [-1, 0, 1], [1, 1, 1]],
                [[0, 0, 2], [0, 1, 2], [-1, 0, 2], [1, 0, 2]],
                [[0, 0, 3], [0, 1, 3], [0, -1, 3], [-1, 1, 3]],
                [[0, 0, 4], [0, 1, 4], [0, -1, 4], [0, -2, 4]],
                [[0, 0, 5], [0, 1, 5], [1, 0, 5], [1, 1, 5]],
                [[0, 0, 6], [0, 1, 6], [0, -1, 6], [1, 1, 6]],
                [[0, 0, 7], [0, 1, 7], [-1, 1, 7], [1, 0, 7]],
            ],
            //次に落ちるブロック
            nextBlocks: [[], [], [], []],
            //今落ちてるブロック
            nowBlocks: [[], [], [], []],
            nowBlocksIndex: [[], [], [], []],
            //キープされたブロック
            keepBlocks: [[0, 0, 0], [0, 0, 0], [0, 0, 0], [0, 0, 0]],
            keepCheck: false,
            keepSecond: true,
            //固定化されたブロック
            allBlocks: [],
            //インターバル解除用
            interval: Number,
            fallCount: 0,
            pushCheck: false,
            number: Number,
            name: '',
            score: 1000,
            time: 0,
            timerInterval: Number,
            startCheck: true,
            endCheck: false,
            stop_count: 0,
            push_down: false,

        }
    },
    mounted() {
        window.addEventListener("keydown", this.keydown_event);
        this.start_game();
    },
    methods: {
        start_game() {
            this.stage = []
            for (let i = 0; i < 24; i++) {
                this.stage.push([])
                for (let j = 0; j < 10; j++) {
                    this.stage[i].push([j, i - 4, 0])
                }
            }
            const newblock = this.defaultBlocks[Math.floor(Math.random() * 7)];
            this.cloneArray(this.nextBlocks, newblock)
            this.score = 0
            this.startCheck = false
            this.endCheck = false
            this.start_turn()
        },
        restart_game() {
            clearInterval(this.interval)
            this.start_game()
        },
        //キーを押したときの処理
        keydown_event(event) {
            //指定キー以外の動きの無効化
            event.preventDefault();
            const downKey = event.keyCode;
            if (!this.pushCheck) {
                return
            }
            // console.log(downKey)
            if (downKey === 37 || downKey === 65) {
                //左
                //左移動
                let moveCheck = true
                for (const i of this.nowBlocksIndex) {
                    if (i[0] - 1 < 0 || this.stage[i[1]][i[0] - 1][2] > 0) {
                        moveCheck = false
                        break
                    }
                }
                if (moveCheck) {
                    for (let i = 0; i < 4; i++) {
                        this.nowBlocks[i][0] -= 40
                        this.nowBlocksIndex[i][0] -= 1
                    }
                }
            } else if (downKey === 38 || downKey === 87) {
                //上
                //右回転
                let moveCheck = true
                const def = this.nowBlocksIndex[0]
                for (let i = 1; i < 4; i++) {
                    const nums = [def[0] + (def[1] - this.nowBlocksIndex[i][1]), def[1] + (def[0] - this.nowBlocksIndex[i][0])]
                    if (this.stage[nums[1]][nums[0]][2] > 0) {
                        moveCheck = false
                        break
                    }
                }
                if (moveCheck) {
                    for (let i = 1; i < 4; i++) {
                        const nums = [def[0] + (def[1] - this.nowBlocksIndex[i][1]), def[1] - (def[0] - this.nowBlocksIndex[i][0])]
                        this.nowBlocksIndex[i][0] = nums[0]
                        this.nowBlocksIndex[i][1] = nums[1]
                        this.nowBlocks[i][0] = this.nowBlocksIndex[i][0] * 40
                        this.nowBlocks[i][1] = (this.nowBlocksIndex[i][1] - 5) * 40 + this.fallCount
                    }
                }
            }
            else if (downKey === 39 || downKey === 68) {
                //右
                //右移動
                let moveCheck = true
                for (const i of this.nowBlocksIndex) {
                    if (i[0] + 1 > 9 || this.stage[i[1]][i[0] + 1][2] > 0) {
                        moveCheck = false
                        break
                    }
                }
                if (moveCheck) {
                    for (let i = 0; i < 4; i++) {
                        this.nowBlocks[i][0] += 40
                        this.nowBlocksIndex[i][0] += 1
                    }
                }
            }
            else if (downKey === 40 || downKey === 83) {
                //下
                //落下
                if (this.push_down) {
                    return
                }
                let max = 24
                for (let i = 0; i < 4; i++) {
                    let high = 0
                    const block = this.nowBlocksIndex[i]
                    while (block[1] + high + 1 < 24 && this.stage[block[1] + high + 1][block[0]][2] === 0) {
                        high += 1
                    }
                    if (max > high) {
                        max = high
                    }
                }
                for (let i = 0; i < 4; i++) {
                    this.nowBlocksIndex[i][1] += max
                    this.nowBlocks[i][1] = this.nowBlocksIndex[i][1] * 40 - 160
                }
                this.push_down = true
                this.pushCheck=false
                this.end_turn()
            }
            else if (downKey === 32) {
                //スペース
                //ブロックのキープ
                if (this.keepSecond) {
                    this.keepSecond = false
                } else {
                    return
                }
                clearInterval(this.interval)
                let middle = [[0, 0, 0], [0, 0, 0], [0, 0, 0], [0, 0, 0]]
                this.cloneArray(middle, this.defaultBlocks[this.nowBlocks[0][2] - 1])
                if (this.keepCheck) {
                    this.cloneArray(this.nowBlocks, this.keepBlocks)
                }
                this.cloneArray(this.keepBlocks, middle)
                this.pushCheck = false
                this.fallCount = 0
                if (this.keepCheck) {
                    this.keep_restart()
                } else {
                    this.keepCheck = true
                    this.start_turn()
                }
            }
        },
        //一巡開始
        start_turn() {
            this.cloneArray(this.nowBlocks, this.nextBlocks)
            this.cloneArray(this.nowBlocksIndex, this.nextBlocks)
            for (let i = 0; i < 4; i++) {
                this.nowBlocks[i][0] = this.nowBlocks[i][0] * 40 + 160
                this.nowBlocks[i][1] = this.nowBlocks[i][1] * 40 - 160
                this.nowBlocksIndex[i][0] += 4
                this.nowBlocksIndex[i][1] += 1
            }
            const newblock = this.defaultBlocks[Math.floor(Math.random() * 7)];
            this.cloneArray(this.nextBlocks, newblock)
            this.interval = setInterval(this.fall, 15)
            this.pushCheck = true
        },
        keep_restart() {
            this.cloneArray(this.nowBlocksIndex, this.nowBlocks)
            for (let i = 0; i < 4; i++) {
                this.nowBlocks[i][0] = this.nowBlocks[i][0] * 40 + 160
                this.nowBlocks[i][1] = this.nowBlocks[i][1] * 40 - 160
                this.nowBlocksIndex[i][0] += 4
                this.nowBlocksIndex[i][1] += 1
            }
            this.interval = setInterval(this.fall, 15)
            this.pushCheck = true
        },
        //ブロックが落ちていく処理
        fall() {
            if (this.fallCount === 40) {
                let downCheck = true
                for (let i = 0; i < 4; i++) {
                    const block = this.nowBlocksIndex[i]
                    if (block[1] + 1 > 23 || this.stage[block[1] + 1][block[0]][2] > 0) {
                        downCheck = false
                        break
                    }
                }
                if (!downCheck) {
                    this.stop_count += 1
                    if (this.stop_count == 30) {
                        this.pushCheck = false
                        this.stop_count = 0
                        this.end_turn()
                    }
                } else {
                    this.push_down = false
                    this.stop_count = 0
                    this.fallChange()
                }
            } else {
                for (let i = 0; i < 4; i++) {
                    const block = this.nowBlocks[i];
                    block[1] += 2
                }
                this.fallCount += 2
            }

        },
        //マス目の入れ替え
        fallChange() {
            this.fallCount = 0
            for (let i = 0; i < 4; i++) {
                const block = this.nowBlocksIndex[i]
                block[1] += 1
            }
        },
        end_turn() {
            this.fallCount = 0
            clearInterval(this.interval)
            setTimeout(this.linecheck, 500)
        },
        linecheck() {
            this.keepSecond = true
            for (let i = 0; i < 3; i++) {
                for (let j = 0; j < 10; j++) {
                    if (this.stage[i][j][2] != 0) {
                        clearInterval(this.interval)
                        return
                    }
                }
            }
            this.pushCheck = false
            // clearInterval(this.interval)
            for (let i = 0; i < 4; i++) {
                const block = this.nowBlocksIndex[i];
                this.stage[block[1]][block[0]][2] = block[2]
                this.nowBlocks[i][2] = 0
            }
            let delLine = []
            for (let i = 4; i < 24; i++) {
                let lineCheck = true
                for (let j = 0; j < 10; j++) {
                    if (this.stage[i][j][2] === 0) {
                        lineCheck = false
                        break
                    }
                }
                if (lineCheck) {
                    delLine.push(i)
                }
            }
            this.score += delLine.length * delLine.length * 100
            for (const index of delLine) {
                for (let i = index; i > 0; i--) {
                    for (let j = 0; j < 10; j++) {
                        this.stage[i][j][2] = this.stage[i - 1][j][2]
                    }
                }
            }
            this.push_down = false
            this.start_turn()
        },
        cloneArray(array1, array2) {
            for (let i = 0; i < 4; i++) {
                for (let j = 0; j < 3; j++) {
                    array1[i][j] = array2[i][j]
                }
            }

        }
    }
}


</script>
<style scoped src="@/styles/tetris.css"></style>
<style src="@/styles/form.css"></style>
