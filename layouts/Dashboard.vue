<template>
    <v-app>
        <div class="dashboard_template">
            <div class="dashboard-main-page" :style="{ background: 'url(' + require('@/assets/images/bg.png') + ') center center no-repeat' }">
                <div class="dloading" v-if="onLoading"><img src="@/assets/images/loading.gif"/></div>
                <div class="main-page-content" v-if="!onLoading">
                    <v-row class="main_dash">
                        <v-col class="sidebar" md="2" style="padding-bottom: 0 !important;">
                            <div class="dsidebar-menu">
                                <DashSidebar />
                            </div>
                        </v-col>
                        <v-col class="main-content" md="10" style="padding-bottom: 0 !important;">
                            <div class="main-content-inner">
                                <!-- <div class="top_content_header">
                                    <PageHeaderBar />
                                </div> -->
                                <router-view />
                            </div>
                        </v-col>
                    </v-row>
                </div>
            </div>
        </div>
    </v-app>
</template>

<script>
    export default {
        name: 'Dashboard',
        data(){
            return {
                onLoading: true
            }
        },
        methods: {
            checkToken(){
                console.log('has login');
                if(localStorage.getItem("token_info") === null){
                    console.log('no token ');
                    this.$router.push('/')
                } else {
                    this.onLoading = false;
                }
            }
        },
        mounted(){
            this.checkToken()
        }
    }
</script>

<style scoped>
    .dashboard-main-page {
        background-size: cover !important;
        min-height: 100vh;
    }
    .dloading {
        text-align: center;
        padding-top: 17%;
    }
    .main-page-content {
        font-family: 'Poppins' !important;
        font-size: 13px;
        font-weight: 400;
        color: #4c5f99;

        max-width: 1440px;
        margin: 0 auto;
    }
    .main-content-inner {
        /* margin-top: 40px; */
        margin-left: 30px;
    }
    .sidebar {
        border-right: 1px solid #cbc9f6;
    }

    .top_content_header {
        text-align: right;
        padding: 20px;
    }

    /* white block design */

    
</style>

<style>
    .wblock > .st-inner {
        background: rgba(255, 255, 255, .72);
        padding: 16px;
        border-radius: 5px;
        width: 98%;
    }

    /* body::-webkit-scrollbar {
        width: 5px;
    } */

    /* body::-webkit-scrollbar-track {
        box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.3);
    }

    body::-webkit-scrollbar-thumb {
        background-color: darkgrey;
        outline: 1px solid slategrey;
    } */
    .main-content-inner {
        margin-top: 25px;
    }
</style>