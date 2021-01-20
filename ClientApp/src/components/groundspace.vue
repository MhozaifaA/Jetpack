<template>
<img draggable="false" class="pic" src="../assets/ground.jpg" />
<img draggable="false" class="pic wave-1" src="../assets/ground1.png" />
<img draggable="false" class="pic wave-2" src="../assets/ground2.png" />
<img draggable="false" class="pic wave-1" src="../assets/ground3.png" />
<img draggable="false" class="pic wave-2" src="../assets/moon1.png" />
<img draggable="false" class="pic wave-3" src="../assets/moon2.png" />

</template>
<script>

    export default {
        name: "groundspace",
    }

</script>


<style scoped>

    .pic {
        position: absolute;
        width: 100%;
        top: 0;
        left: 0;
        object-fit: cover;
        z-index: -1;
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