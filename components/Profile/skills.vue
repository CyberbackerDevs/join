<template>
    <div class="dskills">
        <div class="dskills-inner">
            <div class="dskill-item">
                <div class="dskill-item-inner">
                    <h4>General Skills</h4>
                    <div class="dii-list">
                        <div class="dii-list-item" v-for="(ritem, rkey) in info.general" :key="rkey">
                            <div class="dskill">{{ritem.name}}</div>
                            <div class="drate">
                                <v-rating
                                    color="blue"
                                    length="5"
                                    class="dratestar"
                                    :value="ritem.rate"
                                    half-increments
                                    size="20"
                                ></v-rating>
                            </div>
                            <br class="clear">
                        </div>
                    </div>
                </div>
            </div>
            <div class="dskill-item">
                <div class="dskill-item-inner">
                    <h4>Technical Skills</h4>
                    <div class="dii-list">
                        <div class="dii-list-item" v-for="(ritem, rkey) in info.technical" :key="rkey">
                            <div class="dskill">{{ritem.name}}</div>
                            <div class="drate">
                                <v-rating
                                    color="blue"
                                    length="5"
                                    class="dratestar"
                                    :value="ritem.rate"
                                    half-increments
                                    size="20"
                                ></v-rating>
                            </div>
                            <br class="clear">
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'skills',
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
    .dskill-item {
        background: #fff;
        border-radius: 10px;
        box-shadow: 0px 0px 12px rgb(78 101 167 / 45%);
        overflow: visible;
        position: relative;
        padding: 15px 20px;
        margin-bottom: 15px;
    }
    .dskill-item-inner h4 {
        font-size: 16px;
        line-height: 1em;
        margin-bottom: 16px;
    }
    .dii-list-item {
        border-bottom: 1px solid rgb(76 96 157 / 50%);
        padding: 15px 0;
    }
    .dii-list-item .dskill {
        float: left;
        width: 60%;
    }
    .dii-list-item .drate {
        float: right;
        width: 40%;
    }
    
</style>

<style>
    .dratestar button {
        padding: 0 !important;
    }
    .dratestar {
        text-align: right;
    }
</style>