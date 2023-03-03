<template>
    <div class="notif_main">
        <div class="notif-inner">
            <h2>Welcome back John!</h2>
            <div class="dmessage">
                <div class="dnotifitem"><span>!</span> You need to update your profile</div>
                <div class="dnotifitem"><span>!</span> You need to update your profile</div>
                <div class="dnotifitem"><span>!</span> You need to update your profile</div>
            </div>
            <div class="dmanimage">
                <img src="~/assets/images/woman_purple.png" />
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
    .notif_main {
        background: #fff;
        border-radius: 10px;
        box-shadow: 0px 0px 12px rgb(78 101 167 / 45%);
        position: relative;
    }
    .notif-inner {
        padding: 20px 30px;
        min-height: 160px;
    }
    .notif-inner h2 {
        margin-bottom: 15px;
        line-height: 1em;
        font-weight: 400;
    }
    .dmanimage {
        position: absolute;
        bottom: -1px;
        right: 30px;
    }
    .dmanimage img {
        width: 145px;
        vertical-align: bottom;
    }
    
    .dmessage {
        margin-left: 15px;
    }
    .dnotifitem {
        margin-bottom: 9px;
    }
    .dnotifitem span {
        background: #ff4747;
        color: #fff;
        height: 20px;
        width: 20px;
        text-align: center;
        font-size: 14px;
        display: inline-block;
        border-radius: 50px;
    }
</style>