<template>
    <div class="full_lane">
        <div class="lane">
            <div class="row col0">
                <p>{{ row[0] }}</p>
            </div>
            <div class="row col1">
                <p>{{ row[1] }}</p>
            </div>
            <div class="row col2">
                <p>{{ row[2] }}</p>
            </div>
            <div class="row col3">
                <p>{{ row[3] }}</p>
            </div>
        </div>
        <div class="d-flex justify-space-around button_block">
            <v-btn v-if="spin_check" @click="stop" class="start_button">stop</v-btn>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        spin_check: Boolean,
        lane_num: Number,
    },
    data() {
        return {
            main_num: Number,
            row: ['e', 'r', 'r', 'o', 'r',],
            color: ['', '', '', '', ''],
            rows: ['e', 'r', 'r', 'o', 'r',],
            colors: ['', '', '', '', ''],
            row_default: ['●', '◆', '♥', '▲', '７',],
            color_default: ['yellow', 'blue', 'pink', 'green', 'red'],
            top: ['-200px', '0px', '200px', '400px'],
            top_num: 0,
        }
    },
    mounted() {
        this.start()
        setInterval(this.changer, 1)
    },
    methods: {
        start() {
            let check_array = [true, true, true, true, true,]
            for (let l = 5; l > 0; l--) {
                const num = Math.floor(Math.random() * l)
                let m
                for (m = 0; m < 5; m++) {
                    if (check_array[num + m]) {
                        check_array[num + m] = false
                        break;
                    }
                }
                this.rows[l - 1] = this.row_default[num + m]
                this.colors[l - 1] = this.color_default[num + m]
            }
            this.main_num = Math.floor(Math.random() * 5)
            for (let i = 0; i < 5; i++) {
                const num = (this.main_num + i) % 5
                this.row[i] = this.rows[num]
                this.color[i] = this.colors[num]
            }
        },
        changer() {
            if (this.spin_check || this.top_num != 0) {
                this.top_num -= 10
                if (this.top_num == -200) {
                    this.top_num = 0
                    this.main_num += 1
                    for (let i = 0; i < 5; i++) {
                        const num = (this.main_num + i) % 5
                        this.row[i] = this.rows[num]
                        this.color[i] = this.colors[num]
                    }
                }
                for (let i = 0; i < 5; i++) {
                    this.top[i] = i * 200 + this.top_num + 'px'
                }


            }
        },
        stop() {
            const data = [this.lane_num, this.row[1], this.row[2], this.row[3]]
            this.$emit('add_value', data)
        },
    },
}
</script>

<style scoped>
.lane {
    width: 200px;
    height: 600px;
    border: 1px solid black;
    position: relative;
    overflow: hidden;
}

.button_block {
    height: 80px;
    padding: 20px 0;
    background-color: rgb(0, 201, 0);
}

.row {
    position: absolute;
    width: 200px;
    height: 200px;
}

.row p {
    font-size: 200px;
    line-height: 200px;
    text-align: center;
}

.col0 {
    color: v-bind('color[0]');
    right: 0px;
    top: v-bind('top[0]');
}

.col1 {
    color: v-bind('color[1]');
    right: 0px;
    top: v-bind('top[1]');
}

.col2 {
    color: v-bind('color[2]');
    right: 0px;
    top: v-bind('top[2]');
}

.col3 {
    color: v-bind('color[3]');
    right: 0px;
    top: v-bind('top[3]');
}
</style>