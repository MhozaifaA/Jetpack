<template>
    <h1>This is Game</h1>
    <button class="btn btn-primary" @click="sayMyName">{{btnText}} my name</button>
    <h4>{{myName}}</h4>
</template>

<script>
    export default {
        name: "Jetpack",
        data() {
            return {
                myName: "",
                btnText:"say",
            }
        },

        methods: {
            sayMyName() {
                if (this.btnText == "say") {
                    this.myName = "Jetpack";
                    this.btnText = "hide";
                } else {
                    this.myName = "";
                    this.btnText = "say";
                }
            }
        }
    }
</script>