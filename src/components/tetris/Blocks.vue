<template>
    <MoveBlock v-for="element in blocks" v-bind:block="element" />
</template>
<script>
import MoveBlock from './MoveBlock.vue'
export default {
    props: {
        blocks: Array,
    },
    components:{
        MoveBlock
    }
}
</script>