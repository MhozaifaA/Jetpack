<template>
    <h1>This is Game</h1>
    <button class="btn btn-primary" v-on:click="sayMyName">{{btnText}} my name</button>
    <button class="btn btn-primary" v-on:click="runGame">{{coun}} </button>
    <h4>{{myName}}</h4>
    <input v-model="message" />
    <p v-if="see">{{message}}</p>
    <div class="game">
        <img draggable="false" class="pic" src="../assets/ground.jpg" />
        <img draggable="false" class="pic wave-1" src="../assets/ground1.png" />
        <img draggable="false" class="pic wave-2" src="../assets/ground2.png" />
        <img draggable="false" class="pic wave-1" src="../assets/ground3.png" />
        <img draggable="false" class="pic wave-2" src="../assets/moon1.png" />
        <img draggable="false" class="pic wave-3" src="../assets/moon2.png" />

        <div id="palyer" class="flay" v-bind:style="{top:mantop+'px'}" />

        <!--<div  style="position:absolute; width:100px;height:100px;background:black">

        </div>-->

    </div>


</template>

<script>

    function delay(ms) {
        return new Promise(resolve => setTimeout(resolve, ms));
    }

    function randomNextInt(min, max) {
        return Math.floor(Math.random() * (max - min + 1) + min);
    }


    export default {

        name: "Jetpack",

        created: function () {
            document.title = "Jetpack-game";
            window.addEventListener('keyup', (e) => {
                console.log(e.keyCode); //87   space 32
            });
        },

        data() {
            return {
                myName: "",
                btnText: "say",
                message: "dataa",
                see: true,
                coun: 180,
                mantop: 180,
            }

        },

        computed: {
            // a computed getter
            reversedMessage: function () {
                // `this` points to the vm instance
                return this.message.split('').reverse().join('')
            }
        },

        watch: {
            message: function () {

            },
        },

        methods: {
            runGame: async function () {
                //this.coun = 10;
                //while (this.coun<100) {
                // await delay(1000/90);
                // this.coun++;
                //}
                this.tween(10, 100);
            },

            sayMyName: function () {
                if (this.btnText == "say") {
                    this.myName = "Jetpack";
                    this.btnText = "hide";
                } else {
                    this.myName = "";
                    this.btnText = "say";
                }
                this.see = !this.see;
            },


            tween: function (startValue, endValue) {
                var vm = this
                function animate() {
                    if (TWEEN.update()) {
                        requestAnimationFrame(animate)
                    }
                }
                new TWEEN.Tween({ coun: startValue })
                    .to({ coun: endValue }, 1000)
                    .onUpdate(function () {
                        vm.coun = this.coun.toFixed(0);
                    })
                    .start()
                animate()
            }



        }
    }
</script>


<style scoped>
    * {
        user-select: none;
        user-drag: none;
    }


    #palyer {
        position: absolute;
        height: 150px;
        width: 120px;
        left: 160px;
        top: 180px;
    }

    .flay {
        animation: flay 0.6s linear infinite;
    }

    @keyframes flay {
        0% {
            background: url(../assets/man/flay/1.png);
        }

        20% {
            background: url(../assets/man/flay/1.png);
        }

        40% {
            background: url(../assets/man/flay/2.png);
        }

        60% {
            background: url(../assets/man/flay/3.png);
        }

        80% {
            background: url(../assets/man/flay/4.png);
        }

        100% {
            background: url(../assets/man/flay/5.png);
        }
    }

    .game {
        position: relative;
        width: 1200px;
        height: 560px;
        background: red;
        margin: auto;
        overflow: hidden;
    }

    .pic {
        position: absolute;
        width: 100%;
        top: 0;
        left: 0;
        object-fit: cover;
    }

    .wave-1 {
        animation: wave-1 3s ease-in-out infinite;
    }

    .wave-2 {
        animation: wave-2 3s ease-in-out infinite;
    }

    .wave-3 {
        animation: wave-3 3s ease-in-out infinite;
    }

    @keyframes wave-1 {
        50% {
            top: -20px;
            left: 10px;
        }
    }

    @keyframes wave-2 {
        50% {
            top: -5px;
            left: 20px;
        }
    }

    @keyframes wave-3 {
        50% {
            top: 5px;
            left: 5px;
        }
    }
</style>




<!--console.clear()
new Vue({
  el: "div",
  data() {
    return {
      time: 0,
      isRunning: false,
      interval: null
    }
  },
  methods: {
    toggleTimer() {
      if (this.isRunning) {
        clearInterval(this.interval);
        console.log('timer stops');
      } else {
        this.interval = setInterval(this.incrementTime, 1000);
      }
      this.isRunning = !this.isRunning
    },
    incrementTime() {
      this.time = parseInt(this.time) + 1;
    },
  }
})-->