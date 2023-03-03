<template>
    <div class="attendance_main">
        <div class="am-inner">
            <h3>AwesomeTracker</h3>
             <div class="dops">
            <div class="dopsbuttons">
                <!-- <button v-on:click="dshowdash('old')">Clock In</button> -->
            </div>
            </div>
            
            <div class="dmanimage">
                <img src="~/assets/images/man_purple_phone.png" />
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'notif',
    props: [
        'title',
        'subtitle'
    ],
    data(){
        return {
            
        }
    }
}
</script>

<style scoped>
    .attendance_main {
        background: #fff;
        border-radius: 10px;
        box-shadow: 0px 0px 12px rgb(78 101 167 / 45%);
        width: 95%;
        position: relative;
        padding: 15px 20px;        
        min-height: 130px;
        margin-top: 20px;
    }
    .dmanimage {
        position: absolute;
        bottom: -1px;
        left: 10px;
    }
    .dmanimage img {
        width: 80px;
        vertical-align: bottom;
    }
    .am-inner {
        padding-left: 85px;
        padding-top: 20px;
    }
    .dops {
        margin-bottom: 15px;
    }
    .dops .dopsbuttons {
        display: inline-block;
        width: 60%;
    }
    .dops .dopsbuttons button {
        background: #4d64a7;
        background: linear-gradient(137deg, #485b96 0%, #4d64a7 100%);
        color: #fff;
        line-height: 1em;
        font-size: 11px;
        padding: 10px 20px;
        border-radius: 30px;
    }
    .dops .dopspages {
        display: inline-block;
        width: 39%;
        text-align: right;
        font-size: 12px;
    }
    .dops .dopspages select {
        background: #fff;
        padding: 5px 7px;
        border: 1px solid #4c5f99;
        line-height: 1em;
        border-radius: 5px;
        margin-left: 5px;
        appearance: auto;
    }

</style>