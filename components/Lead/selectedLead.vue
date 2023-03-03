<template>
    <div class="dselectedinner">
        <div class="dheadselectedinfo" :style="{ background: 'url(' + require('@/assets/images/placeheader.jpg') + ') center right no-repeat' }">
            <h2>{{info.name}}</h2>
            <div class="additioanl_details">{{info.status}} | {{info.email}} | {{info.mobile}}</div>
            <div class="dstats">
                <!-- <div class="dsitems">
                    Origin: <span>{{info.origin}}</span>
                </div> -->
                <div class="dsitems">
                    Birthdate: <span>{{info.birthdate}}</span>
                </div>
                <div class="dsitems sendinvite">
                    <PageAddButton label="Send Application invite"/>
                </div>
            </div>
        </div>
        <div class="reply_based" v-if="info.reply != ''">
            <span>Lead Reply:</span>
            {{info.reply}}
        </div>
    </div>
</template>

<script>
import moment from 'moment'

export default {
    name: 'selectedlead',
    props: [
        'info'
    ],
    data(){
        return {
            
        }
    },
    methods:{
        format_date(value){
            if (value) {
                // var newDt = moment(value,"MM/DD/YY")
                let ddate = value.split("-");
                let daterebuild = ddate[2]+"-"+ddate[1]+"-"+ddate[0];
                var newDt = moment(daterebuild,"YY/MM/DD")
                return moment(newDt).format('MMMM D, YYYY')
            }
        },
    }
}
</script>

<style scoped>
    .dheadselectedinfo {
        background-size: cover !important;
        padding: 30px 30px 15px;
        color: #fff;
    }
    .dheadselectedinfo h2 {
        font-size: 26px;
        font-weight: 400 !important;
        line-height: 1em;
        margin-bottom: 5px;
    }
    .dselectedinner {
        border-radius: 5px;
        overflow: hidden;
    }
    .dstats div.dsitems {
        display: inline-block;
        width: 32%;
        vertical-align: middle;
        line-height: 1em;
        font-size: 12px;
        font-weight: 200;
    }

    .dstats div.dsitems span {
        display: block;
        font-weight: 400;
        margin-top: 5px;
    }

    .additioanl_details {
        margin-bottom: 10px;
        line-height: 1em;
        font-size: 14px;
        font-weight: 100;
    }

    .sendinvite {
        text-align: right;
    }

    .reply_based {
        background: #fff;
        padding: 15px 30px;
        font-size: 14px;
        font-style: italic;
    }

    .reply_based span {
        font-weight: bold;
        display: block;
    }

</style>