<template>
    <div class="dprofileinformation">
        <!-- <pre>{{ info }}</pre> -->
        <div class="prof-head" :style="{ background: 'url(' + require('@/assets/images/placeheader.jpg') + ') center right no-repeat' }" v-if="info != []">    
            <div class="prof-head-inner">
                <div class="dprofphoto">
                    <div class="ring-holder">
                        <div class="profimage">
                            <img src="~/assets/images/sample_photo.jpeg" />
                        </div>
                    </div>
                </div>
                <div class="dprofinfo">
                    <div class="dinfo-inner">
                        <h2>{{info.name}}</h2>
                        <div class="dstats">{{info.email}} | {{info.mobile}} | {{info.birthdate}}</div>
                        <div class="dadminrole" v-if="info.pairing.client == 'admin'">Admin | <span>{{info.pairing.cbrole}}</span> | {{info.pairing.paired_date}}</div>
                        <div class="daddress">{{info.address}}</div>
                        <div class="separetor" v-if="info.pairing.client != 'admin'">&nbsp;</div>
                        <div class="dclientsched" v-if="info.pairing.client != 'admin'">
                            <div class="dstatus">Status: <span>{{info.status}} ({{info.start_date}})</span></div>
                            <div class="dsched">Schedule: <span>{{info.schedule}}</span></div>
                            <div class="dupdatesched">
                                <button @click="showEditSchedMenu">Update Schedule</button>
                            </div>
                        </div>
                        <div class="separetor" v-if="info.pairing.client != 'admin'">&nbsp;</div>
                        <div class="doptions">
                            <!-- <button @click="updateRole">Update Role</button> -->
                            <button @click="openChangePosition">Update Positions</button>
                        </div>
                    </div>
                </div>
                <br class="clear"> 
            </div>
        </div>
        <div class="dinfobase">
            <div class="dinfo-inner"> 
                <div class="di-item dschedule" v-if="showEditSched">
                    <div class="di-item-inner">
                        <h3 class="ctitle">Update Schedule</h3>
                        <div class="dsubsmid">
                            <button class="cancelbutton" @click="closeEditSchedMenu">Cancel</button>
                            <button class="savebutton">Save sched</button>
                        </div>
                        <div class="di-itm">
                            <v-menu
                                v-model="showPicker"
                                :close-on-content-click="false"
                                transition="scale-transition"
                                max-width="290px"
                                min-width="290px"
                            >
                                <template v-slot:activator="{ on }">
                                    <label for="">Paired Date</label>
                                    <input class="ddatepick" type="text" v-model="cb_work_info.paired_date" persistent-hint readonly v-on="on">
                                    <!-- <v-text-field
                                        v-model="selectedDate"
                                        label="Birthdate"
                                        persistent-hint
                                        readonly
                                        v-on="on"
                                    ></v-text-field> -->
                                </template>
                                <v-date-picker
                                    v-model="cb_work_info.paired_date"
                                    no-title
                                    @input="showPicker = false"
                                    :show-current="true"
                                ></v-date-picker>
                            </v-menu>
                        </div>
                        <div class="di-itm">
                            <label for="">Roles</label>
                            <div class="drolelist">
                                <v-combobox
                                    v-model="selectedRole"
                                    :items="items"
                                    multiple
                                    chips
                                ></v-combobox>
                            </div>
                        </div>
                        <div class="di-itm fullwidth">
                            <label for="">Scheduled</label>
                            <div class="dstart">
                                <h3>Start of shift</h3>
                                <v-time-picker
                                    v-model="cb_work_info.schedule.start_time"
                                    ampm-in-title
                                    scrollable
                                ></v-time-picker>
                            </div>
                            <div class="dend">
                                <h3>End of shift</h3>
                                <v-time-picker
                                    v-model="cb_work_info.schedule.endtime"
                                    ampm-in-title
                                    scrollable
                                ></v-time-picker>
                            </div>
                        </div>
                    </div>
                </div>

                <div class="di-item update_role" v-if="showUpdateRole">
                    <div class="di-item-inner showpositions">
                        <div class="dii-form">
                            <label for="">Choose a Position</label>
                            <div class="chooseJob">
                                <v-select
                                    @change="changePositions"
                                    item-text="name"
                                    item-value="id"
                                    :items="positions"
                                    dense
                                ></v-select>
                            </div>
                        </div>
                        <div class="dsubsmid">
                            <button class="cancelbutton" @click="closeUpdateRole">Cancel</button>
                            <button class="savebutton" @click='saveNewPosition'>Save Position</button>
                        </div>
                    </div>
                </div>
                
                <div class="duserinfo">
                    <div class="duinfo">
                        <div class="dinfo-left">
                            <h3>Basic Information</h3>
                        </div>
                        <div class="dinfo-right">
                            <button class="editbutton" @click="openUpdateUserInfo">edit</button>
                        </div>
                    </div>
                    <div class="duinfo-items" v-if="!showUpdateUserInfo">
                        show basic information
                    </div>
                    <div class="di-item updateInfo" v-if="showUpdateUserInfo">
                        <!-- <h3>Update Cyberbacker Info</h3> -->
                        <div class="di-uinfo-item dhalf">
                            <v-menu
                                v-model="showPicker"
                                :close-on-content-click="false"
                                transition="scale-transition"
                                max-width="290px"
                                min-width="290px"
                            >
                                <template v-slot:activator="{ on }">
                                    <label for="">Birth Date</label>
                                    <input class="ddatepick" type="text" v-model="cbUserInfo.birthDate" persistent-hint readonly v-on="on">
                                    <!-- <v-text-field
                                        v-model="selectedDate"
                                        label="Birthdate"
                                        persistent-hint
                                        readonly
                                        v-on="on"
                                    ></v-text-field> -->
                                </template>
                                <v-date-picker
                                    v-model="cbUserInfo.birthDate"
                                    no-title
                                    @input="showPicker = false"
                                    :show-current="true"
                                ></v-date-picker>
                            </v-menu>
                        </div>
                        <div class="di-uinfo-item dhalf">
                            <label for="">Mobile Number</label>
                            <input type="text">
                        </div>
                        <div class="di-uinfo-item">
                            <label for="">Address</label>
                            <input type="text">
                        </div>
                        <div class="di-uinfo-item">
                            <label for="">Bio</label>
                            <textarea name="" id="" cols="30" rows="10"></textarea>
                        </div>
                        <div class="dsubsmid">
                            <button class="cancelbutton" @click="closeUpdateInfo">Cancel</button>
                            <button class="savebutton" @click="saveUpdateInfo">Submit</button>
                        </div>
                    </div>
                </div>

                
                <!-- <div class="di-item">
                    <div class="di-item-inner">
                        <h4>Work Experience</h4>
                        <div class="dworkexp">
                            <div class="dworkiitem" v-for="(ritem, rkey) in info.work_experience" :key="rkey" v-bind:class="{ lastitem: rkey==info.work_experience.length-1 }">
                                <div class="dworkinner">
                                    <h4>{{ritem.jobtitle}}</h4>
                                    <div class="dcompany">{{ritem.company}} ({{ritem.month_start}} {{ritem.year_start}} - {{ritem.month_end}} {{ritem.year_end}})</div> 
                                    <div class="dcomaddress">{{ritem.address}}</div>
                                    <div class="respo">{{ritem.responsibility}}</div>
                                </div>
                                
                            </div>
                        </div>
                        <v-timeline dense>
                            <v-timeline-item>timeline item</v-timeline-item>
                            <v-timeline-item class="text-right">
                            timeline item
                            </v-timeline-item>
                            <v-timeline-item>timeline item</v-timeline-item>
                        </v-timeline>
                    </div>-->
                <!-- </div>  -->
                <!-- <div class="di-item">
                    <div class="di-item-inner">
                        <h4>Education</h4>
                        <div class="dlistitems">
                            <div class="educitem">
                                <div class="ei-inner">
                                    <div class="dhead">University</div>
                                    <div class="dschool">{{info.education.college.school}} s.y({{info.education.college.year}})</div>
                                    <div class="ddegree">{{info.education.college.degree}}</div>
                                    <div class="dachivements" v-if="Object.keys(info.education.college.achievements).length !== 0">
                                        <div class="dachivement" v-for="(ritem, rkey) in info.education.college.achievements" :key="rkey">
                                            {{ritem}},
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                -->
            </div>
        </div>
    </div>
    
</template>

<script>
import { print } from 'graphql';
import gql from 'graphql-tag';

export default {
    name: 'profile',
    props: [
        'info',
    ],
    data(){
        return {
            showUpdateUserInfo: false,
            showUpdateRole: false,
            selectedRole: '',
            items: [
                'MAPS',
                'TC',
                'Social Media'
            ],
            positions: [],
            cb_work_info: {
                paired_date: '',
                roles: [],
                schedule: {
                    start_time: null,
                    endtime: null
                }
            },
            cbUserInfo: {
                birthDate: '',
            },
            showPicker: false,
            showEditSched: false,
        }
    },
    methods:{
        showEditSchedMenu(){
            this.showEditSched = true;
        },
        closeEditSchedMenu(){
            this.showEditSched = false;
        },
        closeUpdateRole(){
            this.showUpdateRole = false;
        },
        changePositions(value){
            console.log('selected id', value);
            this.selectedRole = value;
        },
        openChangePosition(){
            console.log('open change Position ');
            this.showUpdateRole = true;
        },
        saveNewPosition(){
            
        },
        closeUpdateInfo(){
            this.showUpdateUserInfo = false;
        },
        openUpdateUserInfo(){
            this.showUpdateUserInfo = true;
        },
        saveUpdateInfo(){
            console.log('save user info');
        },
        updateRole(){
            console.log('add updatre role');
        },
        pullJobs(){
            
        },
        getUserInfo(){
            
        }
    },
    mounted(){
        this.getUserInfo()
        this.pullJobs()
    }
}
</script>

<style scoped>
    .clear {
        clear: both;
    }
    .dprofileinformation {
        background: #fff;
        border-radius: 10px;
        box-shadow: 0px 0px 12px rgb(78 101 167 / 45%);
        overflow: hidden;
    }
    .dprofileinformation .prof-head {
        margin-bottom: 35px;
    }
    .prof-head-inner .dprofphoto {
        float: left;
        width: 20%;
        position: relative;
    }
    .profimage img {
        width: 100%;
        border-radius: 100px;
    }
    .prof-head-inner .dprofinfo {
        float: left;
        width: 65%;
        color: #fff;
        margin-left: 5%;
    }
    .dprofphoto .ring-holder {
        width: 135px;
        height: 135px;
        padding: 10px;
    }
    .dprofphoto .ring-holder::before {
        content: "";
        position: absolute;
        width: 135px;
        height: 135px;
        top: 0px;
        left: 0;
        background: url('~/assets/images/gold_person_big.png') no-repeat center center;
        transform: rotate(30deg);
        background-size: cover !important; 
        transition: all 1s ease-out;
    }
    .dprofphoto .ring-holder:hover::before{
        transform: rotate(-180deg);
        transition: all 1s ease-out;
    }
    
    .prof-head-inner {
        padding: 25px 35px;
    }
    .dinfo-inner h2 {
        font-size: 16px;
        line-height: 1em;
        margin-bottom: 5px;
        font-weight: 400;
    }
    .dinfo-inner .dstats {
        font-size: 13px;
        font-weight: 100;
        line-height: 1em;
        margin-bottom: 5px;
    }
    .dinfo-inner .dbio {
        font-size: 12px;
        font-weight: 100;
        font-style: italic;
    }
    .dinfo-inner .separetor {
        line-height: 1em;
        height: 4px;
        border-radius: 5px;
        background: rgb(254,96,139);
        background: linear-gradient(137deg, rgba(254,96,139,1) 0%, rgba(255,153,162,1) 100%);
        margin: 15px 0;
    }
    .dinfo-inner .dstatus {
        font-weight: 100;
        font-size: 13px;
    }
    .dinfo-inner .dstatus span {
        font-weight: 400;
        text-transform: capitalize;
    }
    .droles .drole {
        display: inline-block;
        margin-right: 5px;
        font-weight: 100;
        font-size: 13px;
    }
    .dprofinfo .dinfo-inner {
        margin-top: 15px;
    }
    .educitem .ei-inner {
        border: 1px solid #e8def7;
        margin-bottom: 10px;
        padding: 15px 20px;
        border-radius: 10px;
        background: #ebe3f9;
        background: linear-gradient(137deg, #ebe3f9 0%, #e8def7 100%);
    }
    .dachivements .dachivement {
        display: inline-block;
        margin-right: 5px;
        font-size: 12px;
        text-transform: capitalize;
    }
    .educitem {
        margin-top: 15px;
    }
    .dworkiitem {
        border-bottom: 1px solid rgb(76 96 157 / 50%);
        padding: 15px 20px;
    }
    .dworkiitem.lastitem {
        border: 0 none;
    }
    .dworkiitem h4 {
        font-size: 16px;
        line-height: 1em;
        margin-bottom: 5px;
    }
    .dworkiitem .dcompany,
    .dworkiitem .dcomaddress {
        line-height: 1em;
        margin-bottom: 5px;
    }
    .dworkiitem .respo {
        line-height: 1.2em;
    }

    .dclientsched {
        position: relative;
    }

    .dupdatesched {
        position: absolute;
        top: 0;
        right: 0;
    }

    .doptions{
        text-align: right;
    }

    .dupdatesched button,
    .doptions button {
        background: #fe608b;
        background: linear-gradient(137deg, #fe608b 0%, #ff99a2 100%);
        line-height: 1em;
        font-size: 12px;
        padding: 10px 20px;
        border-radius: 50px;
    }
    .di-item.dschedule {
        background: #fff;
        border-radius: 10px;
        box-shadow: 0px 0px 12px rgb(78 101 167 / 45%);
        overflow: hidden;
        margin: 20px 30px;
        padding: 20px 30px;
    }
    .di-item.dschedule .di-item-inner {
        position: relative;
    }
    .di-item.dschedule h3.ctitle {
        font-size: 21px;
        margin-bottom: 20px;
        line-height: 1em;
    }
    .di-item.dschedule .dsubsmid {
        position: absolute;
        top: 0;
        right: 0;
    }
    .di-item.dschedule .dsubsmid > button {
        display: inline-block;
    }
    .update_role .dsubsmid {
        text-align: right;
    }
    .update_role .dsubsmid button.savebutton,
    .dinfo-inner .di-item.updateInfo .dsubsmid button.savebutton,
    .di-item.dschedule .dsubsmid button.savebutton {
        background: #fe608b;
        background: linear-gradient(137deg, #fe608b 0%, #ff99a2 100%);
        line-height: 1em;
        font-size: 12px;
        padding: 10px 20px;
        border-radius: 50px;
        color: #fff;
    }
    .update_role .dsubsmid button.cancelbutton,
    .dinfo-inner .di-item.updateInfo .dsubsmid button.cancelbutton,
    .di-item.dschedule .dsubsmid button.cancelbutton {
        background: #4e65a8;
        background: linear-gradient(137deg, #4e65a8 0%, #364a7d 100%);
        line-height: 1em;
        font-size: 12px;
        padding: 10px 20px;
        border-radius: 50px;
        color: #fff;
    }
    .di-item.dschedule .di-itm {
        display: inline-block;
        width: 49%;
        vertical-align: top;
    }
    .di-item.dschedule .di-itm.fullwidth {
        width: 100%;
    }

    .di-item.dschedule .di-itm.fullwidth label {
        display: block;
        width: 100%;
    }
    .di-item.dschedule .di-itm.fullwidth .dstart {
        display: inline-block;
        width: 49%;
    }
    .di-item.dschedule .di-itm.fullwidth .dend {
        display: inline-block;
        width: 49%;
        position: relative;
        vertical-align: top;
    }
    .di-item.dschedule .di-itm.fullwidth .dstart h3,
    .di-item.dschedule .di-itm.fullwidth .dend h3 {
        font-size: 16px;
        line-height: 1em;
        margin-bottom: 10px
    }
    .di-item.dschedule .di-itm label{
        display: block;
        line-height: 1em;
        margin-bottom: 10px;
    }
    .showpositions {
        box-shadow: 0px 0px 12px rgb(78 101 167 / 45%);
        padding: 20px;
        border-radius: 10px;
        margin: 0 4% 3%;
    }
    .dinfo-inner .di-item.updateInfo {
        text-align: center;
        padding: 20px 0;
    }
    .dinfo-inner .di-item.updateInfo h3 {
        text-align: left;
        margin: 0 2% 3%;
        font-size: 18px;
    }
    .dinfo-inner .di-item.updateInfo .di-uinfo-item.dhalf {
        display: inline-block;
        width: 45%;
        margin: 0 2% 3%;
        box-shadow: 0px 0px 12px rgb(78 101 167 / 45%);
        padding: 20px 20px;
        border-radius: 10px;
    }
    .dinfo-inner .di-item.updateInfo .di-uinfo-item {
        box-shadow: 0px 0px 12px rgb(78 101 167 / 45%);
        width: 95%;
        margin: 0 2% 3%;
        padding: 20px 20px;
        border-radius: 10px;
        display: inline-block;
        text-align: left;
    }
    .dinfo-inner .di-item.updateInfo .di-uinfo-item label,
    .dinfo-inner .di-item.updateInfo .di-uinfo-item.dhalf label {
        width: 100%;
        display: block;
        line-height: 1em;
        margin-bottom: 10px;
        font-weight: bold;
    }
    .dinfo-inner .di-item.updateInfo .di-uinfo-item input,
    .dinfo-inner .di-item.updateInfo .di-uinfo-item.dhalf input,
    .dinfo-inner .di-item.updateInfo .di-uinfo-item textarea {
        border-bottom: 1px solid #495f9b;
        width: 100%;
        line-height: 1em;
        font-size: 13px;
        padding: 7px 20px;
    }
    .dinfo-inner .di-item.updateInfo .dsubsmid {
        text-align: right;
    }
    .duserinfo {
        margin: 0 4% 3%;
    }
    .duserinfo .duinfo > .dinfo-left {
        width: 80%;
        display: inline-block;
    }
    .duserinfo .duinfo > .dinfo-right {
        width: 19%;
        display: inline-block;
        text-align: right;
    }
</style>

<style>
    .di-item.dschedule .di-itm.fullwidth .dstart .v-picker__title,
    .di-item.dschedule .di-itm.fullwidth .dend .v-picker__title {
        width: 290px;
        background: #fe608b !important;
        background: linear-gradient(137deg, #fe608b 0%, #ff99a2 100%) !important;
    }
    .di-item.dschedule .di-itm.fullwidth .dstart  .v-picker__body,
    .di-item.dschedule .di-itm.fullwidth .dend  .v-picker__body {
        margin: 0 !important;
    }

    .di-item.dschedule .di-itm.fullwidth .dstart  .v-picker__body .v-time-picker-clock,
    .di-item.dschedule .di-itm.fullwidth .dend  .v-picker__body .v-time-picker-clock {
        background: #fe608b !important;
        background: linear-gradient(137deg, #fe608b 0%, #ff99a2 100%) !important;
        color: #FFf;
    }

    .di-item.dschedule .di-itm .drolelist .v-select--chips {
        margin: 0;
        padding: 0;
    }
    .di-item.dschedule .di-itm .ddatepick {
        border: 1px solid #8d7f8f;
        width: 95%;
        line-height: 1em;
        padding: 10px 25px;
        border-radius: 10px;
    }
</style>