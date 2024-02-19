<template>
    <div class="block"></div>
</template>
<script>

export default {
    props: {
        color: Boolean,
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
.block {
    width: 79.8px;
    height: 30px;
    border: 1px solid white;
    background-color: v-bind(color ? 'green' : 'white');
}
</style>