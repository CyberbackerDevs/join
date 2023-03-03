<template>
    <div class="drefbases">
        <div class="dops">
            <div class="dopsbuttons">
                <button v-on:click="dshowdash('new')">New Referral Dashboard</button>
                <button v-on:click="dshowdash('old')">Old Referral Dashboard</button>
            </div>
            <div class="dopspages" v-if="referralslist.length !== 0">
                <span>Show page</span>
                <select v-on:change="changePage($event)">
                    <option :value="index" v-for="index in max_page" :key="index">{{index}}</option>
                </select>
            </div>
        </div>
        <div class="dash-messages-content">
            <div class="dmc-inner">
                <div class="dtitle">
                    <div class="dt-inner">
                        <h3>{{reftype}} Dashboard Referrals</h3>
                        <div class="sub-title"><strong>{{total_num}}</strong> Referrals from <span v-for="(sitem, sindex) in referrals" :key="sindex">{{sitem}}, </span> </div>
                        <div class="sub-ref-link" v-if="oneRef != ''"><a :href="oneRef" target="_blank">{{oneRef}}</a></div>
                    </div>
                </div>
                <div class="dmhead">
                    <div class="dmh-inner">
                        <div class="s_item s-name">name</div>
                        <div class="s_item s-date">date</div>
                        <div class="s_item s-division">Referred by</div>
                    </div>
                </div>
                <div class="dmcontent">
                    <div class="dmc-inner">
                        <div class="dshowloading" v-if="showLoading">
                            <v-progress-circular indeterminate color="primary"></v-progress-circular>
                        </div>
                        <div class="dshowrefslist" v-if="!showLoading">
                            <div class="dmessageitem" v-for="(item, key) in referralslist" :key="key">
                                <div class="m-item m-name">
                                    <div class="dimage">
                                        <img src="~/assets/images/prof_icon.png" />
                                    </div>
                                    <div class="dname">
                                        <div class="dapname">{{ item.name }}</div>
                                        <div class="dapemail">{{ item.email }}</div>
                                    </div>
                                </div>
                                <div class="m-item m-date">{{ item.created_at }}</div>
                                <div class="m-item m-division">{{ item.referred_by }}</div>
                            </div>
                            <div class="norefs" v-if="referralslist.length === 0">
                                no referrals
                            </div>
                        </div>
                    </div>
                </div>
                <div class="dmhead">
                    <div class="dmh-inner">
                        <div class="s_item s-name">name</div>
                        <div class="s_item s-date">date</div>
                        <div class="s_item s-division">Referred by</div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>

    export default {
        props: [
            'referrals'
        ],
        data () {
            return {
                showLoading: false,
                showBlank: false,
                switch1: true,
                referralslist: [],
                max_page: 0,
                total_num: 0,
                reftype: 'new',
                oneRef: '',
            }
        },
        watch: {
            referrals(){
                this.loadRefs();
            }
        },
        methods: {
            dshowdash(dashboard){
                console.log('change dash ->', dashboard);
                this.reftype = dashboard;
                this.getReferrals(this.referrals, 1, dashboard);
            },
            changePage(event){
                // console.log("show paginate -> ", event.target.value);

                this.getReferrals(this.referrals, event.target.value, this.reftype);
            },
            loadRefs(){
                this.getReferrals(this.referrals, 1, this.reftype);

                console.log('iutems - >', this.referrals.length);

                if(this.referrals.length === 1){
                    this.oneRef = "https://applytocyberbacker.com/assessment/step1?ref="+encodeURI(this.referrals[0].toLowerCase());
                } else {
                    this.oneRef = ''
                }
            },
            getReferrals(token, page = 1, type = "new"){
                let token_info = localStorage.getItem('token_info');

                let self = this;

                let apiHeader = {
                    "Access-Control-Allow-Origin": "http://localhost:3000",
                    "Access-Control-Allow-Methods": "GET, POST, PATCH, PUT, DELETE, OPTIONS",
                    "Access-Control-Allow-Headers": "Origin, Content-Type, X-Auth-Token, Access-Control-*",
                    'token': token_info
                }

                let params = {
                    "token": token,
                    "page": page,
                    "take": 30,
                    "type": type
                }

                this.showLoading = true;

                this.$axios.post('https://be2.applytocyberbacker.com/api/referrals/get/token', params, { headers: apiHeader } )
                .then(function (response) {
                    // self.tokenList = response.data.data.referral;
                    self.referralslist = response.data.data.referral;
                    self.max_page = response.data.data.max_pages;
                    self.total_num = response.data.data.total;
                    if(response.data.data.total == 0){
                        self.showBlank = true;
                    }

                    self.showLoading = false;

                })
                .catch(function (error) {
                    console.log(error);
                });
            },
        },
        mounted(){
            this.getReferrals(this.referrals);
        }
    }
</script>

<style scoped>
    .dash-messages-content {
        background: #fff;
        border-radius: 10px;
        box-shadow: 0px 0px 12px rgb(78 101 167 / 45%);
        overflow: hidden;
        /* width: 95%; */
    }
    .dmhead .s_item {
        display: inline-block;
        padding: 10px 15px;
        line-height: 1em;
        font-weight: bold;
        text-transform: uppercase;
        font-size: 12px;
    }
    .dmcontent .m-item {
        display: inline-block;
        padding: 10px 15px;
        line-height: 1em;
        font-size: 11px;
        font-weight: 400;
        vertical-align: middle;
    }
    .dmhead .s_item.s-name,
    .dmessageitem .m-item.m-name {
        width: 50%;
    }
    .dmhead .s_item.s-title,
    .dmessageitem .m-item.m-title {
        width: 25%;
    }
    .dmhead .s_item.s-date,
    .dmessageitem .m-item.m-date {
        width: 24%;
    }
    .dmhead .s_item.s-division,
    .dmessageitem .m-item.m-division {
        width: 18%;
    }
    .dmh-inner {
        background: #ecddf1;
    }
    .dmessageitem .m-item.m-name .dimage {
        margin-right: 10px;
    }
    .dmessageitem .m-item.m-name .dimage,
    .dmessageitem .m-item.m-name .dname {
        display: inline-block;
        vertical-align: middle;
    }
    .dmessageitem .m-item.m-name .dname .dapname {
        font-weight: bold;
        font-size: 13px;
        line-height: 1.3em;
    }
    .dmessageitem .m-item.m-name .dimage img {
        width: 35px;
        border-radius: 50px;
        border: 2px solid #8092c8;
    }
    .dt-inner {
        padding: 20px 20px 15px;
    }
    .dt-inner h3 {
        line-height: 1em;
        margin-bottom: 3px;
    }
    .dt-inner .sub-title {
        line-height: 1em;
        font-size: 12px;
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
    .norefs {
        text-align: center;
        text-transform: capitalize;
        padding: 15px 0;
        font-weight: bold;
        font-size: 14px;
    }
    .dshowloading {
        text-align: center;
        padding: 50px 0;
    }
    .sub-ref-link a {
        border-radius: 7px;
        display: block;
        text-decoration: none;
        font-size: 12px;
        color: #4c63a5;
        margin-top: 10px;
        border: 1px solid #495c98;
        line-height: 1em;
        padding: 10px 20px;
        background: #495c9840;
    }
</style>