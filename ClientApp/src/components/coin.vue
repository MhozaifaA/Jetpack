<template>
    <div class="coin" v-bind:style="{left:coin.left+'px'}"></div>
</template>

<script>

    export default {
        name: "coin",
        props: {
            coin:Object
        }
    }

</script>

<style scoped>
    .coin {
        position: absolute;
        height: 60px;
        width: 59px;
        top: 460px;
        background: url(../assets/coins/1.png) no-repeat;
        animation: coin 1s linear infinite;
    }


    @keyframes coin {
        0% {
            background: url(../assets/coins/1.png) no-repeat;
        }

        20% {
            background: url(../assets/coins/2.png) no-repeat;
        }

        40% {
            background: url(../assets/coins/3.png) no-repeat;
        }

        60% {
            background: url(../assets/coins/4.png) no-repeat;
        }

        80% {
            background: url(../assets/coins/5.png) no-repeat;
        }

        100% {
            background: url(../assets/coins/6.png) no-repeat;
        }
    }

</style>