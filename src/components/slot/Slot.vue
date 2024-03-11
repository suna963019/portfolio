<template>
    <div class="set_name">
        <h2 class="text-center">スロットゲーム</h2>
        <h3 class="comment">操作説明</h3>
        <table class="comment">
            <tr>
                <th>startボタン</th>
                <td>:</td>
                <td>回転開始</td>
            </tr>
            <tr>
                <th>stopボタン</th>
                <td>:</td>
                <td>回転を一レーンずつ止める</td>
            </tr>
            <tr>
                <th>スペースキー</th>
                <td>:</td>
                <td>回転開始 / 回転を左から一レーンずつ止める</td>
            </tr>
        </table>
    </div>
    <div class="game">
        <div class="d-flex justify-space-between w-600">
            <h2>スコア : {{ score }}</h2>
        </div>
        <div class="slot_box d-flex">
            <Lane v-bind:spin_check="spin_check[0]" v-bind:lane_num="0" v-on:add_value="add_value" ref="lane0" />
            <Lane v-bind:spin_check="spin_check[1]" v-bind:lane_num="1" v-on:add_value="add_value" ref="lane1" />
            <Lane v-bind:spin_check="spin_check[2]" v-bind:lane_num="2" v-on:add_value="add_value" ref="lane2" />
        </div>
        <div v-if="!spin_check_all && chance > 0" class="d-flex justify-center w-600">
            <v-btn @click="spin" class="start_button">start</v-btn>
        </div>
    </div>
</template>

<script>
import Lane from './Lane.vue'

export default {
    data() {
        return {
            score: 0,
            spin_check_all: false,
            spin_check: [false, false, false,],
            values: new Array(9),
            startCheck: true,
            number: 0,
            name: '',
        };
    },
    components: {
        Lane,
    },
    mounted() {
        window.addEventListener('keydown', this.press_key);
        this.spin();
        this.start();
    },
    methods: {
        spin() {
            this.spin_check[0] = true
            this.spin_check[1] = true
            this.spin_check[2] = true
            this.spin_check_all = true
        },
        add_value(array) {
            const num = array[0]
            this.spin_check[num] = false
            this.values[num * 3] = array[1]
            this.values[num * 3 + 1] = array[2]
            this.values[num * 3 + 2] = array[3]
            if (!this.spin_check[0] &&
                !this.spin_check[1] &&
                !this.spin_check[2]) {
                this.endturn()
            }
        },
        endturn() {
            this.spin_check_all = false
            for (let i = 0; i < 3; i++) {
                let check = false
                const start = this.values[i]
                for (let l = -1; l < 2; l++) {
                    if (i + l * 2 < 0 || i + l * 2 > 2) {
                        continue;
                    }
                    if (start === this.values[3 + i + l] && start === this.values[6 + i + l * 2]) {
                        if (start == '７') {
                            this.score += 1000
                        } else {
                            this.score += 200
                        }
                    }
                }
            }
        },
        press_key(event) {
            if (event.code === 'Space' && this.chance > 0) {
                if (!this.spin_check_all) {
                    this.spin()
                } else if (this.spin_check[0]) {
                    this.$refs.lane0.stop()
                } else if (this.spin_check[1]) {
                    this.$refs.lane1.stop()
                } else if (this.spin_check[2]) {
                    this.$refs.lane2.stop()
                }
            }

        },
        restart() {
            this.$refs.lane0.start()
            this.$refs.lane1.start()
            this.$refs.lane2.start()
            this.start()
        },
        start() {
            if (this.name === '') {
                this.name = '名無し'
            }
            this.startCheck = false
            this.chance = 10
            this.score = 0
            this.$refs.lane0.start()
            this.$refs.lane1.start()
            this.$refs.lane2.start()
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
            const response = await fetch('http://54.199.63.195/api/slot/add', param);
            const result = await response.json();
            this.number = result[0]
            this.endCheck = true
        },
    },
}
</script>
<style src="@/styles/form.css"></style>

<style scoped>
.slot_box {
    height: 600px;
    width: 600px;
    background-color: rgb(241, 241, 241);
    border: 1px solid black;
}

.w-600 {
    padding: 20px 0;
    width: 600px;
    height: 80px;
}

h2 {
    margin: auto;
}
</style>