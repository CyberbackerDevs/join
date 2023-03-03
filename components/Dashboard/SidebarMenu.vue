<template>
    <div class="sidebar-menu-main">
        <ul class="main_sidebar_menu">
            <li class="mainitem dashconsts">
                <NuxtLink to="/dashboard">Dashboard</NuxtLink>
            </li>
            <li class="mainitem">
                <div class="dmainsegment">About me</div>
                <ul>
                    <!-- <li><NuxtLink to="/dashboard/profile">Profile</NuxtLink></li> -->
                    <!-- <li><NuxtLink to="/dashboard/messages">Messages</NuxtLink></li> -->
                    <li ><NuxtLink to="/dashboard/referrals">Referrals</NuxtLink></li>
                </ul>
            </li>
            <!-- <li class="mainitem dashconsts">
                <NuxtLink to="/awesomenesstracker">AwesomenessTracker</NuxtLink>
            </li> -->
            <!-- <li class="mainitem" v-if="isAdmin">
                <div class="dmainsegment" >Cyber Backers</div>
                <ul>
                    <li><NuxtLink to="/cyberbackers/assigned">Assigned to me</NuxtLink></li>
                </ul>
            </li> -->
            <!-- <li class="mainitem" v-if="isAdmin">
                <div class="dmainsegment" >Software Backers</div>
                <ul>
                    <li><NuxtLink to="/software/ticket">Ticket</NuxtLink></li>
                </ul>
            </li>
            <li class="mainitem" v-if="isAdmin">
                <div class="dmainsegment" >Quality Control Society</div>
                <ul>
                    <li><NuxtLink to="/software/ticket">Ticket</NuxtLink></li>
                </ul>
            </li> -->
            <!-- <li class="mainitem" v-if="isUserGrowth">
                <div class="dmainsegment">Growth Backer</div>
                <ul>
                    <li><NuxtLink to="/growth/client">Client Profile</NuxtLink></li>
                    <li><NuxtLink to="/growth/allClient">Client List</NuxtLink></li>
                    <li><NuxtLink to="/growth/leads">Growth Source Leads</NuxtLink></li>
                </ul>
            </li> -->
            <li class="mainitem" v-if="isUserHR">
                <div class="dmainsegment">HR Backer</div>
                <ul>
                    <li><NuxtLink to="/hr/applicants">For Expectations Call</NuxtLink></li>
                    <li><NuxtLink to="/hr/allCyberbackers">Cyberbackers</NuxtLink></li>
                </ul>
            </li>
            <!-- <li class="mainitem" v-if="isUserCareer">
                <div class="dmainsegment">Career Gameshows</div>
                <ul>
                    <li><NuxtLink to="/gameshows/all">Gameshows</NuxtLink></li>
                </ul>
            </li> -->
            <li class="mainitem" v-if="isUserCareer">
                <div class="dmainsegment">Career Backer</div>
                <ul>
                    <li><NuxtLink to="/career/leadsv2">Career Source Leads</NuxtLink></li>
                    <li><NuxtLink to="/career/consultant">Career Consultant</NuxtLink></li>
                    <li><NuxtLink to="/career/pool">CB Pool</NuxtLink></li>
                    <li><NuxtLink to="/career/packets">CB Packets</NuxtLink></li>
                    <li><NuxtLink to="/career/analytics">Career Analytics</NuxtLink></li>
                </ul>
            </li>
            <li class="mainitem" v-if="isAdmin">
                <div class="dmainsegment">Settings</div>
                <ul>
                    <li><NuxtLink to="/dashboard/settings">System</NuxtLink></li>
                    <li><NuxtLink to="/dashboard/accountsettings">Account</NuxtLink></li>
                    <!-- <li><NuxtLink to="/dashboard/yassessment">Y Assessment</NuxtLink></li> -->
                </ul>
            </li>
            <li class="mainitem">
                <div class="dmainsegment logoutbutton" v-on:click="logoutUser">Logout</div>
            </li>
        </ul>
    </div>
</template>

<script>
    export default {
        data(){
            return {
                isRole: '',
                isAdmin: true,
                isUserCareer: true,
                isUserHR: true,
                isUserGrowth: false
            }
        },
        methods: {
            getUserRole(){

            },
            // isUserGrowth(){
            //     return false;
            // },  
            // isUserCareer(){
            //     return true;
            // },
            checkUserPermission(){
                let usergroup = localStorage.getItem('token_crl');
                if(usergroup == "cb0"){
                    this.isAdmin = false;
                    this.isUserCareer = false;
                    this.isUserHR = false;
                    this.isUserGrowth = false;
                }
            },
            logoutUser(){
                console.log('logoiut me');

                localStorage.removeItem('token_info');
                localStorage.removeItem('isdate');
                localStorage.removeItem('token_crl');
                this.$router.push('/')
            }
        },
        mounted(){
            this.checkUserPermission();
        }
    }
</script>


<style scoped>
    .sidebar-menu-main ul {
        margin: 0;
        padding: 0;
    }
    .sidebar-menu-main ul li {
        list-style: none;
    }
    .sidebar-menu-main ul li a {
        color: #fff;
    }

    .sidebar-menu-main {
        font-size: 13px;
        font-weight: 400;
        margin-left: 30px;
    }

    .mainitem {
        margin-bottom: 10px;
    }

    .mainitem .dmainsegment {
        font-weight: bold;
        line-height: 1em;
        margin-bottom: 5px;
    }

    .mainitem > ul {
        margin-left: 10px;
    }

    .mainitem > ul li {
        margin-bottom: 6px;
        line-height: 1em;
        display: block;
        transition: all 1s ease-out;
    }

    /* .mainitem > ul li:hover {
        background: rgb(254,96,139);
        background: linear-gradient( 137deg, rgba(254,96,139,1) 0%, rgba(255,153,162,1) 100%);
        transition: all 1s ease-out;
    } */


    .main_sidebar_menu ul li a {
        color: #4c5f99;
        text-decoration: none;
        line-height: 1.6em;
        display: block;
    }

    .main_sidebar_menu ul li a:after,
    .main_sidebar_menu ul li a.nuxt-link-active:after {
        content:"";
        display: block;
        background: rgb(254,96,139);
        background: linear-gradient( 137deg, rgba(254,96,139,1) 0%, rgba(255,153,162,1) 100%);
        height:2px;
        width: 100%;
        bottom: 0;
        opacity: 0;
        transition: all .5s ease-out;    
    }

    .main_sidebar_menu ul li a:hover:after,
    .main_sidebar_menu ul li a.nuxt-link-active:hover:after {
        /* content:""; */
        transition: all .5s ease-out;
        opacity: .4;
        /* display: block;
        height:2px;
        width: 100%;
        bottom: 0; */
    }

    li.mainitem.dashconsts a {
        font-weight: bold;
        line-height: 1em;
        margin-bottom: 5px;
        color: #4c5f99 !important;
        text-decoration: none;
    }
    .mainitem .dmainsegment.logoutbutton {
        cursor: pointer;
    }
</style>