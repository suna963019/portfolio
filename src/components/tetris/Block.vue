<template>
    <div :style="{ top: (block[1] * 40) + 'px', left: (block[0] * 40) + 'px', border: (block[2] === 0 ? 0 : 1) + 'px solid white' }"
        class="block"></div>
</template>
<script>
export default {
    props: {
        block: Array,
    },
    data() {
        return {
            colors: [
                'none',
                'green',
                'purple',
                'blue',
                'aqua',
                'yellow',
                'orange',
                'red',
            ],
            bgColor: 'none'
        }
    },
    watch: {
        block: {
            handler(block) {
                // console.log(this.block)
                this.bgColor = this.colors[block[2]]
            },
            deep: true
        }
    }
}
</script>
<style>
.block {
    height: 40px;
    width: 40px;
    position: absolute;
    z-index: 1;
    background-color: v-bind(bgColor);
}
</style>