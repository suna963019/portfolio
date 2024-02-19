<template>
    <div class="d-flex">
        <div class="set_name">
            <h2 class="text-center">タイピングゲーム</h2>
            <h3 class="comment">操作説明</h3>
            <p class="comment">　英字で文字が出てくるので同じものをキーボードで打ってください。半角全角は区別します。</p>
        </div>
        <div class="content_box">
            <div class="d-flex text">
                <pre class="message1">{{ message1 }}</pre>
                <pre class="now">{{ now }}</pre>
                <pre class="message2">{{ message2 }}</pre>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Type',
    data: function () {
        return {
            default: [
                'spiral staircase',
                'beetle',
                'The City of Ruins',
                'Fig Tart',
                'beetle',
                'The Road to Drorosa',
                'beetle',
                'singularity',
                'Jott',
                'Angel',
                'hydrangea',
                'beetle',
                'singularity',
                'a secret emperor'],
            default_ja: [
                '螺旋階段',
                'カブトムシ',
                '廃墟の町',
                'イチジクのタルト',
                'カブトムシ',
                'ドロローサへの道',
                'カブトムシ',
                '特異点',
                'ジョット',
                'エンジェル',
                '紫陽花',
                'カブトムシ',
                '特異点',
                '秘密の皇帝',
            ],
            count: 0,
            char_count: 1,
            message: '',
            message1: '',
            now: '',
            message2: 'Face the fear, Build the future',
        }
    },
    mounted() {
        window.addEventListener('keypress', this.type)
        this.message1 += this.now
        this.now = String(this.message2).charAt(0)
        this.message2 = String(this.message2).slice(1)
        this.reset()
    },
    methods: {
        type(event) {
            if (this.now == event.key) {
                this.set_next_char()
            }
            if (this.count == 13) {
                this.addData()
                return
            }
            if (this.message2 == '' && this.now === '') {
                this.set_next_string()
            }

        },
        set_next_char() {
            let str = String(this.message2)
            this.message1 += this.now
            this.now = str.charAt(0)
            this.message2 = str.slice(1)
            console.log(this.message2)
        },
        set_next_string() {
            this.count++
            this.message1 = ''
            this.now = this.default[this.count].charAt(0)
            this.message2 = this.default[this.count].slice(1)
            this.message = this.default_ja[this.count]
        },
        reset() {
            this.count = 0
            this.message1 = ''
            this.now = ''
            this.message2 = this.default[0]
            this.message = this.default_ja[0]
            this.set_next_char()
        },
        async addData() {
            this.score = (300 - this.time) * 10
        },
    }
}
</script>

<style scoped>
.content_box {
    width: 500px;
    margin-right: auto;
    position: relative;
}

.text {
    font-size: 48px;
    text-align: justify;
    position: relative;
    padding: 3px;
    border-bottom: 1px solid black;
    letter-spacing: 1px;
    width: 800px;
    margin: auto;
}

.message1 {
    margin-right: -2px;
    background-color: rgb(223, 243, 193);
    color: rgba(0, 128, 0, 0.300)
}

.message2 {
    margin-left: -2px;
    padding-left: 1px;
    background-color: white;
    background-color: rgb(223, 243, 193);
    color: green;
}

.now {
    padding-left: 2px;
    padding-right: 1px;
    background-color: rgb(230, 230, 230);
    color: green;
}
</style>
<style src="@/styles/form.css"></style>