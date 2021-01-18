<template>
    <h1>This Is Demo</h1>
    <p> any keydown will block others -- move wisely <span class="keyBox">{{keydowned}}</span> </p>

    <div class="game">
        <img draggable="false" class="pic" src="../assets/ground.jpg" />
        <img draggable="false" class="pic wave-1" src="../assets/ground1.png" />
        <img draggable="false" class="pic wave-2" src="../assets/ground2.png" />
        <img draggable="false" class="pic wave-1" src="../assets/ground3.png" />
        <img draggable="false" class="pic wave-2" src="../assets/moon1.png" />
        <img draggable="false" class="pic wave-3" src="../assets/moon2.png" />
        <div id="palyer" v-bind:class="changeClass" v-bind:style="{top:player_top+'px',left:player_left+'px'}" />


        <div style="height:100px"></div>

    </div>

</template>

<script>



    function delay(ms) {
        return new Promise(resolve => setTimeout(resolve, ms));
    }

    (function preloadImage() {
        let images = ["../assets/ground1.png", "../assets/ground2.png",
            "../assets/ground3.png", "../assets/moon1.png",
            "../assets/moon2.png", "../assets/man/flay/1.png",
            "../assets/man/flay/2.png",
            "../assets/man/flay/3.png",
            "../assets/man/flay/4.png",
            "../assets/man/flay/5.png",
            "../assets/man/down.png",
            "../assets/man/stand.png",
            "../assets/man/run/1.png",
            "../assets/man/run/2.png",
            "../assets/man/run/3.png",
            "../assets/man/run/4.png",
            "../assets/man/run/5.png",
        ];
        let ims = []
        for (var i in images) {
            var img = new Image();
            img.src = i;
            ims.push(img);
        }
    })()

    //function randomNextInt(min, max) {
    //    return Math.floor(Math.random() * (max - min + 1) + min);
    //}

    export default {

        name: "Jetpack",

        components: {

        },

        data() {
            return {
                player_top: 180,
                player_left: 160,
                isFillDown: true,
                isLockDown: false,
                isLockForward: false,
                isPressDown: false,
                isPressMove: false,
                isForward: false,

                keydowned: "",
            }
        },

        created: function () {
            document.title = "Jetpack-game";
            this.minLeft = 0;
            this.maxLeft = 1000;
            this.maxTop = 0;
            this.backgroundLine = 380;
            this.minTop = this.backgroundLine;

            window.addEventListener('keyup', async () => { this.isPressMove = false; });

            window.addEventListener('keydown', async (e) => {

                //  await delay(100);
                //up 87  down 83   forward 68  backward 65  space 32
                //up 119  down 115   forward 100  backward 97  space 32
                this.keydowned = e.key == " " ? "space" : e.key;

                if (e.keyCode == 87) {
                    this.jumpUp();
                } else
                    if (e.keyCode == 83) {
                        this.isPressDown = true;
                        this.fillDown();
                    }

                if (e.keyCode == 68) {
                    this.isPressMove = true;
                    this.forward();
                } else if (e.keyCode == 65) {
                    this.isPressMove = true;
                    this.backward();
                }

               


            });

        },

        computed: {
            changeClass: function () {
                let className = this.isPressDown ? 'down' : 'flay';
                if (this.player_top == this.backgroundLine) {
                    className = "stand";
                    if (this.isPressMove) {
                        className = "run";
                    }
                }
                return className ;
            }
        },

        watch: {
            //isFillDown() {
            //    console.log("change fill");
            //},
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

                if (this.player_top >= this.backgroundLine) {
                    this.isPressDown = false;
                    return;
                }
                this.isFillDown = true;
                let smooth = 20;
                while (this.isFillDown) {

                    await delay(smooth);
                    this.player_top++;
                    if (this.player_top >= this.backgroundLine) {
                        this.player_top = this.backgroundLine;
                        this.isFillDown = false;
                    }
                    smooth -= (40 / 100);
                }
                this.isLockDown = false;
                this.isPressDown = false;
            },

            async jumpUp() {

                if (this.player_top <= this.maxTop) {
                    return;
                }

                this.isFillDown = false;
                let toTop = this.player_top - 100;
                let smooth = 1;
                while (!this.isFillDown && toTop <= this.player_top) {
                    await delay(smooth);
                    this.player_top--;
                    if (this.player_top <= this.maxTop) {
                        this.player_top = this.maxTop;
                        break;
                    }
                    smooth += (20 / 100);
                }
                if (!this.isLockDown) {
                    this.isLockDown = true;
                    await delay(800);
                    if (this.isFillDown == false) {
                     this.fillDown();
                    }
                }
            },

            async forward() {
                this.isLockForward = false;
                let toLeft = this.player_left + 10;
                while (toLeft >= this.player_left && !this.isLockForward ) {
                    await delay(30);
                    this.player_left++;
                    if (this.player_left >= this.maxLeft) {
                        this.player_left = this.maxLeft;
                        this.backward();
                        break;
                    }
                }

            },

            async backward() {
                this.isLockForward = true;
                let toLeft = this.player_left - 10;
                while (toLeft <= this.player_left && this.isLockForward) {
                    await delay(30);
                    this.player_left--;
                    if (this.player_left <= this.minLeft) {
                        this.player_left = this.minLeft;
                        break;
                    }
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

    .keyBox {
        border-style: solid;
        border-color: darkslategrey;
        border-width: 2px;
        border-radius: 3px;
        padding: 2px;
        color: gray;
        font-weight: bolder;
        text-transform: capitalize;
        margin: 2px;
    }

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
        /*transition: top 0.5s linear;*/
    }

    .down {
        background: url(../assets/man/down.png);
    }

    .flay {
        animation: flay 0.6s linear infinite;
    }

    .run {
        animation: run 0.6s linear infinite;
    }

    .stand {
        background: url(../assets/man/stand.png);
    }

    @keyframes run {
        0% {
            background: url(../assets/man/run/1.png);
        }
        20% {
            background: url(../assets/man/run/2.png);
        }
        40% {
            background: url(../assets/man/run/3.png);
        }
        60% {
            background: url(../assets/man/run/4.png);
        }
        80% {
            background: url(../assets/man/run/5.png);
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
            background: url(../assets/man/run/1.png);
        }
    }

    @keyframes flay {
        0% {
            background: url(../assets/man/flay/1.png);
        }

        20% {
            background: url(../assets/man/flay/2.png);
        }

        40% {
            background: url(../assets/man/flay/3.png);
        }

        60% {
            background: url(../assets/man/flay/4.png);
        }

        80% {
            background: url(../assets/man/flay/5.png);
        }

        100% {
            background: url(../assets/man/flay/1.png);
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

