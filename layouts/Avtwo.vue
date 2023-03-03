<template>
    <div class="d-main-template">
        <div class="d-main-header">
            <img :src="require('@/assets/images/logo_black.png')" alt="">
        </div>
        <div class="dcontentpart">
            <router-view />
        </div>
    </div>
</template>

<script>
    export default {
        name: 'Avtwo'
    }
</script>

<style scoped>
    .d-main-header {
        margin: 40px 50px;
    }
    .d-main-header img {
        width: 250px;
    }
</style>