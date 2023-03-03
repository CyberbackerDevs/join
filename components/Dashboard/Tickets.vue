<template>
    <div class="attendance_main">
        <div class="am-inner">
            <h3>Tickets & Job Orders</h3>
            <div>Coming soon</div>
            <div class="dmanimage">
                <img src="~/assets/images/man_white_beard.png" />
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
        margin-top: 50px;
    }
    .dmanimage {
        position: absolute;
        bottom: -1px;
        right: 10px;
    }
    .dmanimage img {
        width: 68px;
        vertical-align: bottom;
    }
    .am-inner {
        /* padding-left: 85px; */
    }
</style>