<template>
    <div class="main_template" :style="{ background: 'url(' + require('@/assets/images/bluebg.jpg') + ') center center no-repeat' }">
        <div class="dmainlogo">
            <img :src="require('@/assets/images/logo_white.png')" alt="">
        </div>
        <div class="sub-main-logo">
            Start your digital career with us!
        </div>
        <div class="dcontentpart">
            <router-view />
        </div>
    </div>
</template>

<script>
    export default {
        name: 'StageOne'
    }
</script>

<style scoped>
    /* @import url('https://fonts.googleapis.com/css2?family=Lato:wght@100;300;400;700&display=swap'); */

    .dcontentpart {
        width: 1060px;
        margin: 0 auto;
        padding-top: 60px;
        padding-bottom: 130px;
    }
    .dmainlogo {
        text-align: center;
    }

    .dmainlogo img {
        width: 350px;
        padding-top: 130px;
    }

    .sub-main-logo {
        text-align: center;
        color: #fff;
        margin-top: 5px;
    }

    .main_template {
        background-size: cover !important;
        font-family: 'Lato' !important;
        background-attachment: fixed !important;
    }

    @media only screen and (max-width: 1060px) {
        .dcontentpart {
            width: 95%;
        }
    }
</style>