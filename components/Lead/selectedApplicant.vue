<template>
    <div class="dselectedinner">
        <div class="dheadselectedinfo" :style="{ background: 'url(' + require('@/assets/images/placeheader.jpg') + ') center right no-repeat' }">
            <h2>{{info.firstName}} {{info.lastName}}</h2>
            <div class="additioanl_details">{{info.email}} | {{info.mobile}} | {{info.birthdate}}</div>
            <div class="dstats">
                <div class="dsitems">
                    Status: <span>{{info.status}}</span>
                </div>
                <!-- <div class="dsitems">
                    Birthdate: <span>{{info.birthdate}}</span>
                </div> -->
                <div class="dsitems sendinvite" v-if="info.status == 'ExpectationCall'">
                    <div class="dbutton" v-if="!showExpCallLoading">
                        <PageAddButton v-on:click.native="sendExpectationsCall" label="Send Expectation Call"/>
                    </div>
                    <div class="showloading" v-if="showExpCallLoading">
                        <v-progress-circular
                            indeterminate
                            color="amber"
                        ></v-progress-circular>
                    </div>
                </div>
            </div>
        </div>
        <div class="additional_info">
            <div class="ai-inner">
                <div class="show_conf_expcall" v-if="showConfirmExpectationCall">
                    <div class="sce_inner">
                        <button class="dshowinvite" @click="confirmExpectationCall(info.id)">Confirm Expectation Call Invite</button>
                        <button class="dshowcancel" @click="cancelExpectationCall">Cancel Expectation Call Invite</button>
                    </div>
                </div>
                <div class="dshowoptions" v-if="info.status == 'ExpectationCallSent' && !showOptionConfirm">
                    <div class="dso_inner">
                        <button class="goactivate" @click="activateApplicant(info.id)">Activate Applicant</button>
                        <button class="godeactivate" @click="deactivateApplicant(info.id)">Deactivate Applicant</button>
                    </div>
                </div>
                <div class="sconfirmoptions" v-if="showOptionConfirm">
                    <div class="sc-inner">
                        <h3>Confirm {{opt_status}} Applicant</h3>
                        <div class="doptionbuttons" v-if="!showOptbuttons">
                            <button class="goactivate" @click="confirmConfOption(info.id)">Confirm</button>
                            <button class="godeactivate" @click="cancelConfOption">Cancel</button>
                        </div>
                        <div class="doptloading" v-if="showOptbuttons">
                            <v-progress-circular
                                indeterminate
                                color="primary"
                            ></v-progress-circular>
                        </div>
                    </div>
                </div>
                <div class="ai-item" v-if="info.expected_call_date != '' && info.activated_date == '' && info.deactivated_date == ''">
                    <div class="dbleft">
                        <PageAddButton label="Activate Applicant"/>
                    </div>
                    <div class="dbright">
                        <PageAddButton label="Deactivate Applicant"/>
                    </div>
                </div>
                <div class="ai-item" v-if="info.expected_call_date != ''">
                    <h2>Date of Interview</h2>
                    <div class="dvals">{{info.expected_call_date}}</div>
                </div>
                <div class="ai-item">
                    <h2>Consultant</h2>
                    <div class="dvals">{{info.consultant}}</div>
                </div>
                <div class="ai-item">
                    <h2>Position that they are applying for*</h2>
                    <div class="dvals">
                        <div class="positions_list">
                            <div v-for="posti in position" :key="posti.id">
                                {{ posti.name }}
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import { print } from 'graphql';
import gql from 'graphql-tag';

export default {
    name: 'selectedlead',
    props: [
        'info'
    ],
    data(){
        return {
            opt_status: '',
            selected_option: '',
            showOptbuttons: false,
            showOptionConfirm: false,
            showConfirmExpectationCall: false,
            showExpCallLoading: false,
            position: []
        }
    },
    methods: {
        activateApplicant(id){
            console.log('activate ->', id);
            this.opt_status = 'Activating';
            this.selected_option = 'Activated';
            this.showOptionConfirm = true;
        },
        deactivateApplicant(id){
            console.log('deactivate ->', id);
            this.opt_status = 'Deactivating';
            this.selected_option = 'Deactivated';
            this.showOptionConfirm = true;
        },
        cancelConfOption(){
            this.opt_status = '';
            this.showOptionConfirm = false;
        },
        confirmConfOption(id){
            console.log('selected option ->', this.selected_option);
            this.showOptbuttons = true;
            

            let self = this;
            
            const UPDATETOEXPECTATIONCALL = gql`
                mutation updateAccountDetails($accountId:UUID!, $ystatus:AccountStatus!){
                    updateAccountDetails(accountId: $accountId){
                        updateStatus(status:$ystatus)
                    }
                }
            `;

            const headers = {
                'Access-Control-Allow-Origin': '*',
            }

            this.$axios.post('https://cyberbacker-ms-api.herokuapp.com/graphql', {
                query: print(UPDATETOEXPECTATIONCALL),
                variables: {
                    accountId: id,
                    ystatus: this.selected_option
                },
            },
            {
                headers: headers
            })
            .then(function (response) {
                window.location.reload()
            })
            .catch(function (error) {
                console.log('show error ->', error);
            });


        },
        confirmExpectationCall(id){
            console.log('confirm expectation call ->', id);
            this.showConfirmExpectationCall = false;

            let self = this;
            let assesment_status = 'ExpectationCallSent';
            const UPDATETOEXPECTATIONCALL = gql`
                mutation updateAccountDetails($accountId:UUID!, $ystatus:AccountStatus!){
                    updateAccountDetails(accountId: $accountId){
                        updateStatus(status:$ystatus)
                    }
                }
            `;

            const headers = {
                'Access-Control-Allow-Origin': '*',
            }

            this.$axios.post('https://cyberbacker-ms-api.herokuapp.com/graphql', {
                query: print(UPDATETOEXPECTATIONCALL),
                variables: {
                    accountId: id,
                    ystatus: assesment_status
                },
            },
            {
                headers: headers
            })
            .then(function (response) {
                window.location.reload()
            })
            .catch(function (error) {
                console.log('show error ->', error);
            });
        },
        cancelExpectationCall(){
            console.log('cancel expectation call');
            this.showExpCallLoading = false;
            this.showConfirmExpectationCall = false;
        },
        sendExpectationsCall(){
            console.log('Send expectations call');
            this.showExpCallLoading = true;
            this.showConfirmExpectationCall = true;

        },
        getYassessmentValues(uuid){
            console.log('get y assessment values ->', uuid);
            let self = this;
            let bearertoken = localStorage.getItem('token_info');

            const GETASSESSMENT = gql`
                query getAccountById($accountId:UUID!){
  					getAccountById(id:$accountId){
                        assessment{
                            aesthetic
                            economic
                            individualistic
                            political
                            altruist
                            regulatory
                            theoretical
                        }
                        jobPositionsApplied{
                            name
                        }
                    }
                }
            `;

            const headers = {
                'Access-Control-Allow-Origin': '*',
                'Authorization': bearertoken
            }

            this.$axios.post('https://cyberbacker-ms-api.herokuapp.com/graphql', {
                query: print(GETASSESSMENT),
                variables: {
                    accountId: uuid
                },
            },
            {
                headers: headers
            })
            .then(function (response) {
                console.log('responses -> ', response.data.data.getAccountById);
                self.y_assessment = response.data.data.getAccountById.assessment;
                self.position = response.data.data.getAccountById.jobPositionsApplied;
                // self.showLoading = false;
            })
            .catch(function (error) {
                console.log('show error ->', error);
            });



        }
    },
    mounted(){
        this.getYassessmentValues(this.info.id);
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

    .additional_info {
        background: #fff;
        padding: 15px 30px;
        font-size: 14px;
        font-style: italic;
    }

    .ai-item {
        margin-bottom: 25px;
    }

    .ai-item h2 {
        font-size: 14px;
        line-height: 1em;
        margin-bottom: 10px;
        font-style: normal;
    }

    .ai-item .dvals {
        font-size: 12px;
        font-style: normal;
    }

    .ai-item .dbleft,
    .ai-item .dbright{
        width: 49%;
        display: inline-block;
    }
    .ai-item .dbleft {
        text-align: right;
    }
    .positions_list {
        margin-bottom: 25px;
    }
    .positions_list > div {
        font-size: 11px;
        line-height: 1em;
        border: 1px solid #4d62a3;
        display: inline-block;
        padding: 10px 15px;
        color: #4d62a3;
        margin-right: 10px;
        border-radius: 15px;
        margin-bottom: 15px;
    }
    .dstats div.dsitems.sendinvite {
        width: 67%; 
    }
    .show_conf_expcall {
        text-align: center;
        margin-bottom: 20px;
    }
    .show_conf_expcall button.dshowinvite,
    .goactivate {
        background: #fe608b;
        background: linear-gradient(137deg, #fe608b 0%, #ff99a2 100%);
        color: #fff;
        font-weight: 400;
        padding: 8px 20px;
        border-radius: 26px;
        text-transform: capitalize;
        font-size: 12px;
        margin: 0 5px;
    }
    .show_conf_expcall button.dshowcancel,
    .godeactivate {
        background: #485a98;
        background: linear-gradient(137deg, #485a98 0%, #4d65a6 100%);
        color: #fff;
        font-weight: 400;
        padding: 8px 20px;
        border-radius: 26px;
        text-transform: capitalize;
        font-size: 12px;
        margin: 0 5px;
    }
    .dshowoptions {
        margin-bottom: 20px;
    }
    .dshowoptions {
        text-align: center;
    }
    .sconfirmoptions h3 {
        text-align: center;
        font-style: initial;
        font-weight: 400;
        
    }
    .sconfirmoptions h3 {
        text-align: center;
        font-style: initial;
        font-weight: 400;
        margin-top: -33px;
        position: absolute;
        left: 22%;
        background: #fff;
        padding: 0 15px;
    }

    .sconfirmoptions{
        text-align: center;
        margin-bottom: 20px;
        border: 2px solid #4c609f;
        padding: 20px;
        position: relative;
        border-radius: 10px;
    }
</style>