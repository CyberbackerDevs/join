<template>
    <div class="login_template">
        <div class="login-main-page">
            <router-view />
        </div>
    </div>
</template>

<script>
    export default {
        name: 'Login'
    }
</script>

<style>
    /* @import url('https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;700&display=swap'); */

    body {
        background: #f5f7fd;
        font-family: 'Roboto', sans-serif;
    }
</style>