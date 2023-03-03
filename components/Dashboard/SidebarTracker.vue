<template>
    <div class="tracker-menu-main">
        <div class="tr-inner">
            <div class="dworknow">
                <button>
                    i am ready to work!
                </button>
            </div>
        </div>
    </div>
</template>

<script>

    export default {
        data () {
            return {
                switch1: true,
            }
        },
    }
</script>

<style scoped>
    .tracker-menu-main {
        margin-bottom: 25px;
    }
    .tr-inner .v-application--wrap {
        flex: none;
    }
</style>