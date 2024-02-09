<template>
    <div :style="{ top: (block[1]) + 'px', left: (block[0]) + 'px', backgroundColor: colors[block[2]], border: [block[2] === 0 ? 0 : 1] + 'px solid white' }"
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
                'black',
                'green',
                'purple',
                'blue',
                'aqua',
                'yellow',
                'orange',
                'red',
            ],
        }
    },
}
</script>
<style>
.block {
    height: 40px;
    width: 40px;
    position: absolute;
    z-index: 3;
}
</style>