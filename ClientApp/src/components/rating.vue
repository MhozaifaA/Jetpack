<template>
    <span v-for="item in count" v-bind:key="item" :class="kind"></span>
</template>


<script>
    export default {
        name:"rating",
        props: {
            kind:String,
            count: Number,
        }
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

</style>