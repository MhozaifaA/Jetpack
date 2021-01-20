<template>
    <div  class="fireball" :style="{top:fire.top+'px',left:fire.left+'px'}"></div>
</template>

<script>

    export default {

        name: "fireball",
        props: {
            fire: Object,
        }

    }

</script>

<style scoped>
    .fireball {
        position: absolute;
        transform: scaleX(-1);
        width: 130px;
        height: 50px;
        padding: 0;
        background-size: 100% 100% !important;
        background: url(../assets/fire.gif) no-repeat;
    }
    /*512 197  2.5989*/
</style>