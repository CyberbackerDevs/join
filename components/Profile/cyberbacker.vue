<template>
    <div class="dclient">
        <div class="dclient-inner" v-for="(ritem, rkey) in info" :key="rkey">
            <div class="dcontent">
                <div class="dclient-info-inner">
                    <div class="dclientprof">
                        <div class="cprofinner">
                            <img src="~/assets/images/prof_icon.png" />
                        </div>
                    </div>
                    <div class="ddetails">
                        <h3>{{ritem.name}}</h3>
                        <div class="cemail">{{ritem.email}}</div>
                        <!-- <div class="cphone">{{ritem.phone}}</div> -->
                        <div class="croles">
                            <div class="drole" v-for="(roleitem, rolekey) in ritem.role" :key="rolekey">
                                {{roleitem}},
                            </div>
                        </div>
                        <div class="cstartdate">Start Date: <span>{{ritem.start_date}}</span></div>
                        <div class="cschedule">Schedule: <span>{{ritem.schedule}}</span></div>
                    </div>
                    <br class="clear">
                    <div class="dleavemessage">
                        <NuxtLink to="/dashboard/profile">Cyberbacker Profile</NuxtLink>
                        <!-- <button>Cyberbacker Profile</button> -->
                    </div>
                </div>
            </div>
            <div class="dimae">
                <img src="~/assets/images/woman_white.png" />
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'client',
    props: [
        'info',
    ],
    data(){
        return {
            
        }
    }
}
</script>

<style scoped>
    .clear {
        clear: both;
    }
    .dclient-inner {
        background: #fff url('~/assets/images/gold.png') no-repeat center left;
        border-radius: 10px;
        box-shadow: 0px 0px 12px rgb(78 101 167 / 45%);
        overflow: visible;
        position: relative;
        margin-bottom: 30px;
    }
    .dclient-inner .dimae {
        position: absolute;
        bottom: 0;
        right: -20px;
    }
    .dclient-inner .dimae img {
        width: 105px;
        vertical-align: bottom;
    }
    .dclient-info-inner {
        padding: 15px 20px;
    }
    .dclient-info-inner h3 {
        font-size: 16px;
        font-weight: 400;
        line-height: 1em;
        margin-bottom: 5px;
    }
    .dclient-info-inner .cemail {
        font-size: 12px;
        line-height: 1em;
    }
    .dclient-info-inner .cphone {
        font-size: 12px;
        line-height: 1em;
        margin-bottom: 5px;
    }
    .dclient {
        margin-bottom: 20px;
    }
    .dclientprof {
        width: 60px;
        margin-bottom: 5px;
        float: left;
    }
    .dclientprof img {
        width: 100%;
        border-radius: 100px;
    }
    .ddetails {
        float: left;
        margin-left: 10px;
    }
    .dleavemessage button,
    .dleavemessage a {
        background: #4c63a3;
        background: linear-gradient(137deg, #4c63a3 0%, #364474 100%);
        color: #fff;
        padding: 10px 15px;
        line-height: 1em;
        border-radius: 50px;
        margin-top: 10px;
        display: inline-block;
        text-decoration: none;
    }
    .croles .drole {
        display: inline-block;
        font-weight: bold;
    }
    .cstartdate {
        font-size: 12px;
        line-height: 1em;
        margin-bottom: 2px;
    }
    .cschedule {
        font-size: 12px;
        line-height: 1em;
        margin-bottom: 2px;
    }
    
</style>