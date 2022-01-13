<template>
  
    <div>
        <label class="mx-2 font-weight-bold">Enemies:{{killedEnemies}}</label>
        <label class="mx-2 font-weight-bold">Coins:{{coinsplayer}}</label>
    </div>
    <p> any keydown will block others -- move wisely  try Key 'G' <span class="keyBox">{{keydowned}}</span> </p>


    <rating :count="countdie" kind="hart"></rating>
    <br>
    <rating :count="power" kind="power"></rating>

    <h2 v-if="countdie==0">You Lost</h2>
    <button v-if="countdie==0" class="btn btn-outline-success my-1" @click="restart">Play</button>

    <div class="game" v-bind:class="losteffect">

        <groundspace />

        <player :player_top="player_top" :player_left="player_left" :changeClass="changeClass" />


        <enemy v-for="enemy in enemies" v-bind:key="enemy.id" :enemy="enemy">
            <rating :count="enemy.hart" kind="star"></rating>
        </enemy>

        <coin v-for="coin in coins" v-bind:key="coin" :coin="coin"></coin>

        <fireball v-for="fire in fires" v-bind:key="fire" :fire="fire" />

    </div>

</template>

<script>
    import rating from "./rating"
    import groundspace from "./groundspace"
    import player from "./player"
    import fireball from "./fireball"
    import enemy from "./enemy"
    import coin from "./coin"


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
            groundspace,
            player,
            rating,
            fireball,
            enemy,
            coin
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
                losteffect:"",
                countdie: 5,
                enemies: [],
                fires: [],
                enemyKey: 0,
                isLost: false,
                killedEnemies:0,
                coinsplayer:0,
                coins: [],
                sound: true,
                power:5,
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


            window.addEventListener('keyup', (e) => {
                if (this.isLost) return;
                this.isPressMove = false;
                if (e.keyCode == 71 && this.power>0) {
                    this.fireRain();
                    this.power--;
                }
            });

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
                if (this.isLost) {
                    className = "die";
                }
                return className;
            }
        },

        watch: {
            countdie() {
                this.losteffect = "hart-effect";
                if (this.isLost) {
                    this.losteffect = "redshadow";
                } else {
                    setTimeout(() => {
                        this.losteffect = "";
                        if (this.isLost) {
                            this.losteffect = "redshadow";
                        }
                    }, 2000);
                }
            },
          
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
                let _speed = randomNextInt(1, 10);
                switch (num) {
                    case 1:
                        return { id: this.enemyKey++, top: _top, left: 1200, size: 120, class: "enemy-1", speed:_speed, hart: 4 };
                    case 2:
                        return { id: this.enemyKey++, top: _top, left: 1200, size: 100, class: "enemy-2", speed: _speed,  hart: 3 };
                    case 3:
                        return { id: this.enemyKey++, top: _top, left: 1200, size: 80, class: "enemy-3", speed: _speed,  hart: 2 };
                    case 4:
                        return { id: this.enemyKey++, top: _top, left: 1200, size: 60, class: "enemy-4", speed: _speed,  hart: 1 };
                    default:
                        return { id: this.enemyKey++, top: 0, left: 1200, size: 120, class: "enemy-moon", speed: _speed,  hart: 0 };
                }
            },

            createCoins() {
                let isCreate = randomNextInt(0, 1000) > 995;
                const offset = 70;
                let nextStartleft = 1200;
                if (this.coins.length > 0) {
                    let last = this.coins[this.coins.length-1];
                    if (last.left>1200) {
                        nextStartleft = last.left + 2 * offset;
                    }
                }
                if (isCreate) {
                    let numofcoin = randomNextInt(1, 10);
                    for (var i = 0; i < numofcoin; i++) {
                        this.coins.push({ left: nextStartleft + ((i) * offset) });
                    }
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

                this.createCoins();

                //coin move
                for (let i = 0; i < this.coins.length; i++) {
                    this.coins[i].left -= 3;

                    if (this.coins[i].left <= -60) {
                        const index = this.coins.indexOf(this.coins[i]);
                        if (index > -1) {
                            this.coins.splice(index, 1);
                        }
                        break;
                    }

                    let offset = 30;
                    let player = { x: this.player_left + offset, y: this.player_top + offset, height: this.player_height - offset, width: this.player_width - offset };
                    let coin = { x: this.coins[i].left, y: 460, height: 60, width: 59 };

                    if (isInterscect(player, coin)) {
                        this.coinsplayer++;
                        const index = this.coins.indexOf(this.coins[i]);
                        if (index > -1) {
                            this.coins.splice(index, 1);
                        }
                    }
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

                            if (--this.enemies[j].hart == 0) {
                                this.killedEnemies++;
                                const index = this.enemies.indexOf(this.enemies[j]);
                                if (index > -1) {
                                    this.enemies.splice(index, 1);
                                }
                            }

                            const f_index = this.fires.indexOf(this.fires[i]);
                            if (f_index > -1) {
                                this.fires.splice(f_index, 1);
                            }
                            break;
                        }
                    }
                }

                //enemy move
                for (let i = 0; i < this.enemies.length; i++) {
                    this.enemies[i].left -= this.enemies[i].speed;

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

                    if (this.enemies[i].left <= -this.enemies[i].size) {
                       // this.enemies.shift();
                        const index = this.enemies.indexOf(this.enemies[i]);
                        if (index > -1) {
                            this.enemies.splice(index, 1);
                        }
                    }

                }

                if (!this.isLost)
                    window.requestAnimationFrame(this.runEnemies);
            },

            shoot() {
                this.fires.push({ top: this.player_top + 40, left: this.player_left + 10 });
            },
            fireRain() {
                for(let  i=0 ;i<25;i+=1)
                {
                    this.fires.push({ top: 20*i + 0, left: 10 });
                }
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
                let toLeft = this.player_left + 15;
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
                this.coins = [];
                this.enemyKey = 0;
                this.caltoCreatEnemy = 0;
                this.pointtoCreatEnemy = 120;
                this.isLost = false;
                this.killedEnemies = 0;
                this.coinsplayer = 0;
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

  

   

    * {
        user-select: none;
        user-drag: none;
    }


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

   
    .game {
        position: relative;
        width: 1200px;
        height: 560px;
        margin: auto;
        overflow: hidden;
        box-shadow: inset 0 0 0px red;
    }
    .redshadow {
        box-shadow: inset 0 0 30px red;
        animation: depthlosteffect 2s linear infinite;
    }
    .game.hart-effect {
        animation: losteffect 2s ease 1;
    }

    @keyframes depthlosteffect {
        50% {
            box-shadow: inset 0 0 80px red;
        }
       
    }

    @keyframes losteffect {
        50% {
            box-shadow: inset 0 0 40px red;
        }
    }

</style>

