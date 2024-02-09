<template>
    <p>●</p>
</template>
<script>

export default {
    props: {
        ball: Object,
    }
}
</script>
<style scoped>
p {
    left: v-bind(ball[2] - 8 + 'px');
    top: v-bind(ball[3] - 14 + 'px');
    position: absolute;
    width: 32px;
    height: 32px;
    font-size: 32px;
    line-height: 32px;
    color: red;
    z-index: 1;
}
</style>