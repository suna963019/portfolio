<template>
    <div class="ball_box">
        <Ball :speedx="-15" :speedy="30" />
        <Ball :speedx="20" :speedy="40"/>
        <Ball :speedx="10" :speedy="20"/>
        <Ball :speedx="-23" :speedy="0" />
        <Ball :speedx="24" :speedy="20"/>
        <Ball :speedx="12" :speedy="10"/>
        <Ball :speedx="-3" :speedy="13" />
        <Ball :speedx="4" :speedy="0"/>
        <Ball :speedx="8" :speedy="100"/>
    </div>
</template>

<script>
import Ball from './Ball.vue';
import Wall from './Wall.vue'
export default {
    name: 'Pati',
    components: {
    Wall,
    Ball
},
}
</script>

<style>
.ball_box {
    border: 1px solid black;
    margin: 100px auto 0px;
    width: 516px;
    height: 697px;
    position: relative;
}

</style>