<template>
    <div class="dprofileinformation">
        <div class="prof-head" :style="{ background: 'url(' + require('@/assets/images/placeheader.jpg') + ') center right no-repeat' }">    
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
                        <h2>Sample Person</h2>
                        <div class="dstats">{{info.email}} | {{info.contact}} | {{info.state}}, {{info.county}}</div>
                        <div class="dctinfo">
                            <div class="di-item-inner">
                                <div class="dinfoleft">
                                    <div class="dil-inner">
                                        <h4>MSF: </h4>
                                        <div class="di-vals">{{info.msf}}</div>
                                    </div>
                                </div>
                                <div class="dinforight">
                                    <div class="dir-inner">
                                        <h4>ETF: </h4>
                                        <div class="di-vals">{{info.etf}}</div>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <div class="separetor">&nbsp;</div>
                        <div class="sbio">{{info.bio}}</div>
                    </div>
                </div>
                <br class="clear"> 
            </div>
        </div>
        <div class="dinfobase">
            <div class="dinfo-inner">
                <div class="di-item">
                    <div class="di-item-inner">
                        <h4>Business Info</h4>
                        <div class="dbusinessinfo">
                            <div class="db-inner">
                                <div class="bsname">{{info.business.name}}</div>
                                <div class="bsaddress">{{info.business.address}}</div>
                                <div class="bsbio">{{info.business.descriptiom}}</div>
                                <div class="bsindustry">{{info.business.industry}}</div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="di-item">
                    <div class="di-item-inner">
                        <h4>Timeline</h4>
                        <div class="dtimeline">
                            <div class="dt-inner">
                                <v-timeline dense class="dclient-timeline">
                                    <v-timeline-item>
                                        <div class="dtime-inner">
                                            <h3>Sign Contract</h3>
                                            <div class="dvals">October 3, 2021</div>
                                            <div class="ddesc">Sign with Growth</div>
                                        </div>
                                    </v-timeline-item>
                                    <v-timeline-item>
                                        <div class="dtime-inner">
                                            <h3>Assigned a Cyberbacker</h3>
                                            <div class="dvals">October 3, 2021</div>
                                            <div class="ddesc">Assigned Alma Buenaventura to Client</div>
                                        </div>
                                    </v-timeline-item>
                                    <v-timeline-item>
                                        <div class="dtime-inner">
                                            <h3>Contract Ended with CB</h3>
                                            <div class="dvals">March 3, 2022</div>
                                            <div class="ddesc">Contract Ended with Alma Buenaventura</div>
                                        </div>
                                    </v-timeline-item>
                                    <v-timeline-item>
                                        <div class="dtime-inner">
                                            <h3>Assigned a Cyberbacker</h3>
                                            <div class="dvals">October 3, 2021</div>
                                            <div class="ddesc">Assigned Lynne Egonio to Client</div>
                                        </div>
                                    </v-timeline-item>
                                </v-timeline>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'client_info',
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
    .dprofileinformation {
        background: #fff;
        border-radius: 10px;
        box-shadow: 0px 0px 12px rgb(78 101 167 / 45%);
        overflow: hidden;
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

    .dinfo-inner .di-item {
        padding: 10px 30px;
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

    .db-inner {
        margin-left: 20px;
    }
    .db-inner .bsname {
        font-weight: bold;
        font-size: 13px;
        line-height: 1em;
        margin-bottom: 5px;
    }
    .db-inner .bsaddress {
        font-size: 13px;
        line-height: 1em;
        margin-bottom: 5px;
    }
    .db-inner .bsbio {
        line-height: 1em;
        font-size: 13px;
        margin-bottom: 5px;
    }
    .db-inner .bsindustry {
        line-height: 1em;
        font-size: 13px;
        margin-bottom: 5px;
    }
    .di-item-inner h4 {
        margin-bottom: 10px;
    }
    .dctinfo .dil-inner h4 {
        display: inline-block;
        margin: 0;
        line-height: 1em;
    }
    .dctinfo .dil-inner .di-vals {
        display: inline-block;
    }
    .dctinfo .dir-inner h4 {
        display: inline-block;
        margin: 0;
        line-height: 1em;
    }
    .dctinfo .dir-inner .di-vals {
        display: inline-block;
    }
    .dctinfo .dinfoleft,
    .dctinfo .dinforight {
        display: inline-block;
    }
</style>