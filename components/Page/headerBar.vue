<template>
    <div class="header_bar">
        <div class="dheadprofimage">
            <img src="~/assets/images/sample_photo.jpeg" @click="openOptions" />
        </div>
        <div class="dshowoptions" v-if="isOptionOpen">
            <div class="optioninner">
                <ul>
                    <li><NuxtLink to="/dashboard/profile">Profile</NuxtLink></li>
                    <li><a href="">Logout</a></li>
                </ul>
            </div>
        </div>
        <br class="clear">
    </div>
</template>

<script>
export default {
    name: 'headerbar',
    props: [
    ],
    data(){
        return {
            isOptionOpen: false
        }
    },
    methods: {
        openOptions(){
            if(this.isOptionOpen){
                this.isOptionOpen = false;
            } else {
                this.isOptionOpen = true;
            }
        }
    }
}
</script>

<style scoped>
    .clear {
        clear: both;
    }
    .dheadprofimage {
        width: 60px;
        float: right;
    }
    .dheadprofimage img {
        width: 100%;
        border-radius: 50px;
        border: 3px solid rgb(52 67 110 / 70%);
        cursor: pointer;
    }
    .header_bar {
        position: relative;
    }
    .dshowoptions {
        position: absolute;
        right: 0;
        top: 70px;
        background: #fff;
        border-radius: 10px;
        box-shadow: 0px 0px 12px rgb(78 101 167 / 45%);
        overflow: hidden;
        padding: 10px 20px;
        width: 180px;
        z-index: 9;
    }
    .dshowoptions ul{   
        margin: 0;
        padding: 0;
        border-bottom: 1px solid #dedbf1;
    }
    .dshowoptions ul li {
        list-style: none;
    }
    .dshowoptions ul li a {
        display: block;
        text-decoration: none;
        color: #4c5f99;
        line-height: 1em;
        border-top: 1px solid #dedbf1;
        padding: 5px 0px;
        font-size: 14px;
        font-weight: 600;
    }
</style>