<template>
    <h1>This is Game</h1>

    <div class="game">
        <img draggable="false" class="pic" src="../assets/ground.jpg" />
        <img draggable="false" class="pic wave-1" src="../assets/ground1.png" />
        <img draggable="false" class="pic wave-2" src="../assets/ground2.png" />
        <img draggable="false" class="pic wave-1" src="../assets/ground3.png" />
        <img draggable="false" class="pic wave-2" src="../assets/moon1.png" />
        <img draggable="false" class="pic wave-3" src="../assets/moon2.png" />

        <div id="palyer" v-bind:class="changeClass" v-bind:style="{top:player_top+'px',left:player_left+'px'}" />

    </div>

</template>

<script>

    function delay(ms) {
        return new Promise(resolve => setTimeout(resolve, ms));
    }

    //function randomNextInt(min, max) {
    //    return Math.floor(Math.random() * (max - min + 1) + min);
    //}


    export default {

        name: "Jetpack",

        data() {
            return {
                player_top: 180,
                player_left: 160,
                isFillDown: true,
                isLockDown: false,
                isPressDown: false,
                isForward: false,
            }
        },

        created: function () {
            document.title = "Jetpack-game";

            window.addEventListener('keydown', (e) => {

                 //up 87  down 83   forward 68  backward 65  space 32
                 //up 119  down 115   forward 100  backward 97  space 32

                console.log(e.keyCode);
                if (e.keyCode == 87) {
                    this.jumpUp();
                } else
                    if (e.keyCode == 83) {
                    this.isPressDown = true;
                    this.fillDown();
                }

                if (e.keyCode == 68) {
                    this.forward();
                } else if (e.keyCode == 65) {
                    this.backward();
                }



            });

        },
    
        computed: {
            changeClass: function () {
                return this.isPressDown ? 'down' : 'flay';
            }
        },

        watch: {
            isFillDown() {
                console.log("change fill");
            },
        },

        methods: {

            engine() {
                this.fillDown();
                this.jumpUp();
                this.forward();
                this.backward();
            },

            runGame: async function () {
             
            },

            async fillDown() {
                if (this.player_top >= 380) {
                    this.isPressDown = false;
                    return;
                }
                this.isFillDown = true;
                let smooth = 20;
                while (this.isFillDown) {
                    await delay(smooth);
                    this.player_top++;
                    if (this.player_top >= 380 ) {
                        this.isFillDown = false;
                    }
                    smooth -= (40 / 100);
                }
                this.isLockDown = false;
                this.isPressDown = false;
            },

            async jumpUp() {

                if (this.player_top <= 0) {
                    return;
                }

                this.isFillDown = false;
                let toTop = this.player_top - 100;
                let smooth = 1;
                while (!this.isFillDown && toTop <= this.player_top) {
                    await delay(smooth);
                    this.player_top--;
                    if (this.player_top <= 0) {
                        break;
                    }
                    smooth+=(20/100);
                }
                if (!this.isLockDown) {
                    this.isLockDown = true;
                    await delay(100);
                    this.fillDown();
                }
            },

            async forward() {
                let toLeft = this.player_left + 10;
                while (toLeft >= this.player_left) {
                    await delay(30);
                    this.player_left++;
                }
            },

              async backward() {
                let toLeft = this.player_left - 10;
                while (toLeft <= this.player_left) {
                    await delay(30);
                    this.player_left--;
                }
            }


        },

        mounted() {
            this.fillDown();
           // window.requestAnimationFrame(this.engine());
        },
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

    .down {
        background: url(../assets/man/down.png);
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