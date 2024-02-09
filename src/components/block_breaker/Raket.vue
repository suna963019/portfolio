<template>
    <div class="raket"></div>
</template>
<script>

export default {
    props: {
        block:Number,
    },
    data() {
        return {
        }
    },
    mounted() {
    }
}
</script>
<style>
.raket {
    position: absolute;
    top: 650px;
    left: v-bind(block+'px');
    width: 100px;
    height: 4px;
    background-color: black;
}
</style>