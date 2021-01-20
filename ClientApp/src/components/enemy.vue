<template>
    <div v-bind:style="{top:enemy.top+'px',left:enemy.left+'px',width:enemy.size+'px',height:enemy.size+'px'}"
         v-bind:class="enemy.class">
        <div style="margin-top:-30px">
            <slot></slot>
        </div>
    </div>
</template>

<script>

    export default {
        name: "enemy",
        props: {
            enemy: Object
        }
    }

</script>

<style scoped>

    .enemy-1 {
        position: absolute;
        height: 120px;
        width: 120px;
        background: url(../assets/enemy/1.png) no-repeat;
    }

    .enemy-2 {
        position: absolute;
        height: 100px;
        width: 100px;
        background: url(../assets/enemy/2.png) no-repeat;
    }

    .enemy-3 {
        position: absolute;
        height: 80px;
        width: 80px;
        background: url(../assets/enemy/3.png) no-repeat;
    }

    .enemy-4 {
        position: absolute;
        height: 60px;
        width: 60px;
        background: url(../assets/enemy/4.png) no-repeat;
    }



    .enemy-moon {
        position: absolute;
        height: 120px;
        width: 120px;
        background: url(../assets/enemy/moon.png) no-repeat;
    }

</style>