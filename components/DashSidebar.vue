<template>
    <div class="main-sidebar">
        <div class="dlogohere">
            <div class="dlogoinner">
                <img src="~/assets/images/logo_black.png" />
            </div>
        </div>
        <!-- Awesomeness Tracker -->
        <!-- <DashboardSidebarTracker /> -->

        <!-- Dashboard sidebar menu -->
        <DashboardSidebarMenu />
    </div>
</template>


<style scoped>
    .main-sidebar {
        min-height: 100vh;
        /* background: #404e68; */
        color: #4c5f99;
    }
    .dlogoinner img {
        width: 100%;
    }
    .dlogohere {
        margin: 25px 0;
    }
</style>