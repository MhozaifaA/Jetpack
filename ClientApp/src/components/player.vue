<template>
    <div id="palyer" v-bind:class="changeClass" v-bind:style="{top:player_top+'px',left:player_left+'px'}" />
</template>




<script>
    export default {
        name: "player",
        props: {
            player_top: Number,
            player_left: Number,
            changeClass: String
        },
    }
</script>

<style scoped>
    #palyer {
        position: absolute;
        height: 150px;
        width: 120px;
        left: 160px;
        top: 180px;
        /*transition: top 0.5s linear;*/
    }

    .down {
        background: url(../assets/man/down.png) no-repeat;
    }

    .flay {
        animation: flay 0.6s linear infinite;
    }

    .run {
        animation: run 0.6s linear infinite;
    }

    .stand {
        background: url(../assets/man/stand.png) no-repeat;
    }

    .shootflay {
        animation: shootflay 0.6s linear infinite;
    }

    .die {
        animation: die 1s linear infinite;
    }

    @keyframes die {
        0% {
            background: url(../assets/man/die/7.png) no-repeat;
        }

        33.3% {
            background: url(../assets/man/die/8.png) no-repeat;
        }

        66.6% {
            background: url(../assets/man/die/9.png) no-repeat;
        }

        100% {
            background: url(../assets/man/die/10.png) no-repeat;
        }
    }


    @keyframes shootflay {
        0% {
            background: url(../assets/man/shootflay/1.png) no-repeat;
        }

        25% {
            background: url(../assets/man/shootflay/2.png) no-repeat;
        }

        50% {
            background: url(../assets/man/shootflay/3.png) no-repeat;
        }

        75% {
            background: url(../assets/man/shootflay/4.png) no-repeat;
        }

        100% {
            background: url(../assets/man/shootflay/1.png) no-repeat;
        }
    }

    @keyframes run {
        0% {
            background: url(../assets/man/run/1.png) no-repeat;
        }

        20% {
            background: url(../assets/man/run/2.png) no-repeat;
        }

        40% {
            background: url(../assets/man/run/3.png) no-repeat;
        }

        60% {
            background: url(../assets/man/run/4.png) no-repeat;
        }

        80% {
            background: url(../assets/man/run/5.png) no-repeat;
        }
        /* 50% {
            background: url(../assets/man/run/6.png);
        }
        60% {
            background: url(../assets/man/run/7.png);
        }
        70% {
            background: url(../assets/man/run/8.png);
        }
        80% {
            background: url(../assets/man/run/9.png);
        }
        90% {
            background: url(../assets/man/run/10.png);
        }*/
        100% {
            background: url(../assets/man/run/1.png) no-repeat;
        }
    }

    @keyframes flay {
        0% {
            background: url(../assets/man/flay/1.png) no-repeat;
        }

        20% {
            background: url(../assets/man/flay/2.png) no-repeat;
        }

        40% {
            background: url(../assets/man/flay/3.png) no-repeat;
        }

        60% {
            background: url(../assets/man/flay/4.png) no-repeat;
        }

        80% {
            background: url(../assets/man/flay/5.png) no-repeat;
        }

        100% {
            background: url(../assets/man/flay/1.png) no-repeat;
        }
    }
</style>