<template>
    <div class='sk-double-bounce'>
        <div class='sk-child sk-double-bounce-1'></div>
        <div class='sk-child sk-double-bounce-2'></div>
    </div>
</template>

<script>
    export default {
        name: "Loader",
    }
</script>

<style scoped>
    .sk-double-bounce {
        width: 4em;
        height: 4em;
        position: relative;
        margin: auto;
    }
    .sk-double-bounce .sk-child {
        width: 100%;
        height: 100%;
        border-radius: 50%;
        background-color: #b745b6;
        opacity: 0.6;
        position: absolute;
        top: 0;
        left: 0;
        -webkit-animation: sk-double-bounce 2.0s infinite ease-in-out;
        animation: sk-double-bounce 2.0s infinite ease-in-out;
    }
    .sk-double-bounce .sk-double-bounce-2 {
        -webkit-animation-delay: -1.0s;
        animation-delay: -1.0s;
    }

    @-webkit-keyframes sk-double-bounce {
        0%, 100% {
            -webkit-transform: scale(0);
            transform: scale(0);
        }
        50% {
            -webkit-transform: scale(1);
            transform: scale(1);
        }
    }

    @keyframes sk-double-bounce {
        0%, 100% {
            -webkit-transform: scale(0);
            transform: scale(0);
        }
        50% {
            -webkit-transform: scale(1);
            transform: scale(1);
        }
    }
</style>