<template>
    <p> any keydown will block others -- move wisely <span class="keyBox">{{keydowned}}</span> </p>
    <span v-for="h in countdie" v-bind:key="h" class="hart"></span>
    <h2 v-if="countdie==0">You Lost</h2>
    <button v-if="countdie==0" class="btn btn-outline-success my-1" @click="restart">Play</button>

    <div class="game">
        <img draggable="false" class="pic" src="../assets/ground.jpg" />
        <img draggable="false" class="pic wave-1" src="../assets/ground1.png" />
        <img draggable="false" class="pic wave-2" src="../assets/ground2.png" />
        <img draggable="false" class="pic wave-1" src="../assets/ground3.png" />
        <img draggable="false" class="pic wave-2" src="../assets/moon1.png" />
        <img draggable="false" class="pic wave-3" src="../assets/moon2.png" />
        <div id="palyer" v-bind:class="changeClass" v-bind:style="{top:player_top+'px',left:player_left+'px'}" />

       
           
        <div v-for="enemy in enemies" v-bind:key="enemy.id"
             v-bind:style="{top:enemy.top+'px',left:enemy.left+'px',width:enemy.size+'px',height:enemy.size+'px'}"
             v-bind:class="enemy.class">
            <div style="margin-top:-30px"><span v-for="h in enemy.hart" v-bind:key="h" class="star"></span></div>
        </div>

      

        <div v-for="fire in fires" v-bind:key="fire" class="fireball" :style="{top:fire.top+'px',left:fire.left+'px'}"></div>
    </div>

</template>
<!--<div v-for="enemy in enemies" v-bind:key="enemy.id"
      v-bind:style="{top:enemy.top+'px',left:enemy.left+'px',width:enemy.size+'px',height:enemy.size+'px'}"
      v-bind:class="enemy.class"> </div>-->
<script>



    function delay(ms) {
        return new Promise(resolve => setTimeout(resolve, ms));
    }

    function isInterscect(rectA, rectB) {
        return !(rectA.x + rectA.width < rectB.x ||
            rectB.x + rectB.width < rectA.x ||
            rectA.y + rectA.height < rectB.y ||
            rectB.y + rectB.height < rectA.y);
    }

    //(function preloadImage() {
    //    let images = ["../assets/ground1.png", "../assets/ground2.png",
    //        "../assets/ground3.png", "../assets/moon1.png",
    //        "../assets/moon2.png", "../assets/man/flay/1.png",
    //        "../assets/man/flay/2.png",
    //        "../assets/man/flay/3.png",
    //        "../assets/man/flay/4.png",
    //        "../assets/man/flay/5.png",
    //        "../assets/man/down.png",
    //        "../assets/man/stand.png",
    //        "../assets/man/run/1.png",
    //        "../assets/man/run/2.png",
    //        "../assets/man/run/3.png",
    //        "../assets/man/run/4.png",
    //        "../assets/man/run/5.png",
    //    ];
    //    let ims = []
    //    for (var i in images) {
    //        var img = new Image();
    //        img.src = i;
    //        ims.push(img);
    //    }
    //})() //demons song

    function randomNextInt(min, max) {
        return Math.floor(Math.random() * (max - min + 1) + min);
    }

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
                countdie: 5,
                enemies: [],
                fires: [],
                enemyKey: 0,
            }
        },

        created: function () {
            document.title = "Jetpack-game";
            this.minLeft = 0;
            this.maxLeft = 1000;
            this.maxTop = 0;
            this.backgroundLine = 380;
            this.minTop = this.backgroundLine;
            this.player_width = 120;
            this.player_height = 150;
            /////*****const

            this.caltoCreatEnemy = 0;
            this.pointtoCreatEnemy = 120;
            this.isLost = false;

            window.addEventListener('keyup', (e) => {
                if (this.isLost) return;
                this.isPressMove = false;
                if (e.keyCode == 32) {
                    this.shoot();
                }
            });

            window.addEventListener('keydown', async (e) => {
                if (this.isLost) return;
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

                    //  className = "shootflay";
                }



                return className;
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

            createEnemy(num) {
                // let min_top = 10; 30   420
                let _top = randomNextInt(30, 420);
                switch (num) {
                    case 1:
                        return { id: this.enemyKey++, top: _top, left: 1200, size: 120, class: "enemy-1", hart: 4 };
                    case 2:
                        return { id: this.enemyKey++, top: _top, left: 1200, size: 100, class: "enemy-2", hart: 3 };
                    case 3:
                        return { id: this.enemyKey++, top: _top, left: 1200, size: 80, class: "enemy-3", hart: 2 };
                    case 4:
                        return { id: this.enemyKey++, top: _top, left: 1200, size: 60, class: "enemy-4", hart: 1 };
                    default:
                        return { id: this.enemyKey++, top: 0, left: 1200, size: 120, class: "enemy-moon", hart: 0 };
                }
            },

            runEnemies() {

                this.caltoCreatEnemy++;
                if (this.enemies.length == 0) {
                    this.enemies.push(this.createEnemy(randomNextInt(1, 4)));
                }
                if (this.caltoCreatEnemy >= this.pointtoCreatEnemy) {
                    this.enemies.push(this.createEnemy(randomNextInt(1, 4)));
                    this.pointtoCreatEnemy = this.caltoCreatEnemy + randomNextInt(50, 100);
                }

                //fire move


                for (let i = 0; i < this.fires.length; i++) {
                    this.fires[i].left += 10;

                    if (this.fires[i].left >= this.maxLeft) {
                        this.fires.shift();
                        break;
                    }
                    let offset = 30;
                    for (let j = 0; j < this.enemies.length; j++) {

                        let fire = {
                            x: this.fires[i].left, y: this.fires[i].top,
                            height: 50, width: 130
                        };

                        let enemy = {
                            x: this.enemies[j].left + offset, y: this.enemies[j].top + offset,
                            height: this.enemies[j].size - offset, width: this.enemies[j].size - offset
                        };
                        if (isInterscect(fire, enemy)) {

                            if (--this.enemies[j].hart==0) {
                                const index = this.enemies.indexOf(this.enemies[j]);
                                if (index > -1) {
                                    this.enemies.splice(index, 1);
                                }
                            }

                            const f_index = this.fires.indexOf(this.fires[j]);
                            if (f_index > -1) {
                                this.fires.splice(f_index, 1);
                            }
                            break;
                        }
                    }
                }

                //enemy move
                for (let i = 0; i < this.enemies.length; i++) {
                    this.enemies[i].left -= 2;

                    let offset = 30;
                    let player = { x: this.player_left + offset, y: this.player_top + offset, height: this.player_height - offset, width: this.player_width - offset };
                    let enemy = { x: this.enemies[i].left + offset, y: this.enemies[i].top + offset, height: this.enemies[i].size - offset, width: this.enemies[i].size - offset };

                    if (isInterscect(player, enemy)) {
                        this.backward(true);
                        this.countdie--; //lost
                        if (this.countdie == 0) {
                            this.isLost = true;
                        }
                    }

                    if (this.enemies[i].left == -this.enemies[i].size) {
                        this.enemies.shift();
                    }

                }


                if (!this.isLost)
                    window.requestAnimationFrame(this.runEnemies);
            },

            shoot() {
                this.fires.push({ top: this.player_top + 40, left: this.player_left + 10 });
            },

            helpfillDown(smooth) {
                this.player_top += smooth;
                if (this.player_top >= this.backgroundLine) {
                    this.player_top = this.backgroundLine;
                    this.isFillDown = false;
                }

                if (this.isFillDown) {
                    smooth += 0.2;
                    window.requestAnimationFrame(() => this.helpfillDown(smooth));
                } else {
                    this.isLockDown = false;
                    this.isPressDown = false;
                }
            },

            fillDown() {

                if (this.player_top >= this.backgroundLine) {
                    this.isPressDown = false;
                    return;
                }
                this.isFillDown = true;
                this.helpfillDown(1);
            },

            async helpjumpUp(toTop, smooth) {
                this.player_top -= smooth;
                let breaked = false;
                if (this.player_top <= this.maxTop) {
                    this.player_top = this.maxTop;
                    breaked = true;
                }
                if (!this.isFillDown && toTop <= this.player_top && !breaked) {
                    smooth -= smooth * 3 / 100;
                    window.requestAnimationFrame(() => this.helpjumpUp(toTop, smooth));
                } else {
                    if (!this.isLockDown) {
                        this.isLockDown = true;
                        await delay(800);
                        if (this.isFillDown == false) {
                            this.fillDown();
                        }
                    }
                }
            },

            jumpUp() {

                if (this.player_top <= this.maxTop) {
                    return;
                }

                this.isFillDown = false;
                let toTop = this.player_top - 100;
                this.helpjumpUp(toTop, 5);
            },

            helpforward(toLeft) {
                this.player_left++;
                if (this.player_left >= this.maxLeft) {
                    this.player_left = this.maxLeft;
                    this.backward();
                    return;
                }
                if (toLeft >= this.player_left && !this.isLockForward) {
                    window.requestAnimationFrame(() => { this.helpforward(toLeft); });
                }
            },

            forward() {
                this.isLockForward = false;
                let toLeft = this.player_left + 10;
                this.helpforward(toLeft);
            },

            helpbackward(toLeft, speed) {
                this.player_left -= speed;
                if (this.player_left <= this.minLeft) {
                    this.player_left = this.minLeft;
                    return;
                }
                if (toLeft <= this.player_left && this.isLockForward)
                    window.requestAnimationFrame(() => { this.helpbackward(toLeft, speed); });
            },

            backward(force) {
                this.isLockForward = true;
                let toLeft = this.player_left - 10;
                let speed = 1;
                if (force === true) {
                    toLeft = this.minLeft;
                    speed = 10;
                    this.isForward = false;
                    this.isFillDown = false;
                    this.isPressDown = false;
                    this.isPressMove = false;
                }
                this.helpbackward(toLeft, speed);

            },

            restart() {
                this.player_top = 180;
                this.player_left = 160;
                    this.isFillDown = true;
                    this.isLockDown = false;
                    this.isLockForward = false;
                    this.isPressDown = false;
                    this.isPressMove = false;
                    this.isForward = false;
                    this.keydowned = "";
                    this.countdie = 5;
                    this.enemies = [];
                    this.fires = [];
                    this.enemyKey = 0;
                    this.caltoCreatEnemy = 0;
                this.pointtoCreatEnemy = 120;
                this.isLost = false;
                this.fillDown();
                this.runEnemies();
            },

        },

        mounted() {

            this.fillDown();
            this.runEnemies();
        },


    }
</script>


<style scoped>

    .hart::after {
        content: '❤';
        color: red;
        font-size: 24px;
    }
    .star:after {
        content: '⭐';
        color: darkgoldenrod;
        font-size: 16px;
    }

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

    .enemy-moon {
        position: absolute;
        height: 120px;
        width: 120px;
        background: url(../assets/enemy/moon.png) no-repeat;
    }

    /*  @keyframes spin {
        100% {
            transform: rotate(360deg);
        }
    }*/

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

