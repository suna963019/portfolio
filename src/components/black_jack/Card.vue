<template>
    <div>
        <p>{{ cardData.type }}</p><p>{{ cardData.num }}</p>
    </div>
</template>
<script>

export default {
    props:{
        cardData:Object,
    },
    data(){
        return{

        }
    },
}
</script>
<style scoped>
div{
    height: 200px;
    width: 150px;
    margin: 30px;
    padding:65px 30px;
    display: flex;
    justify-content: space-between;
    background-color: white;
    border-radius: 10px;
    border: 4px solid ;
    border-color: v-bind(cardData.color);
    
}
p{
    width: 60px;
    text-align: center;
    font-size: 40px;
    color: v-bind(cardData.color);
}
</style>