<template>
    <div class="dtrio">
        <div class="dtrio-inner">
            <div class="dtio-item">
                <div class="dfrom">{{info.assignment}}</div>
                <div class="dinfobase" v-if="withCareer">
                    <div class="dimage">
                        <div class="trprofinner">
                            <img src="~/assets/images/prof_icon.png" />
                        </div>
                    </div>
                    <div class="tinfo">
                        <!-- <div class="dnname">{{info.name}}</div> -->
                        <div class="dfname">{{info.fname}}</div>
                        <div class="demail">{{info.email}}</div>
                        <div class="dcontact">{{info.contact}}</div>
                    </div>
                    <br class="clear">
                    <div class="dbutton">
                        <div class="dleftitem dzoom">
                            <button>Launch Zoom</button>
                        </div>
                        <div class="drightitem dmessage">
                            <button>Drop a Message</button>
                        </div>
                        <br class="clear">
                    </div>
                </div>
                <div class="dinfobase" v-if="!withCareer">
                    <div class="dif-inner">
                        <div class="dic-inner" v-if="!showBackerList">
                            <button class="pairme" @click="addCareer"><span>+</span> Pair with Career</button>
                        </div>
                        <div class="add-client" v-if="showBackerList">
                            <div class="ac-client-list">
                                <div class="searchpart">
                                    <input type="text" v-model="findcb" name="searchc" placeholder="Find Career" v-on:keyup="findcbnow">
                                </div>
                                <div class="client_list">
                                    <div class="cl_inner">
                                        <div class="cl-item" v-for="(item, key) in careerbackers.slice(0, 10)" :key="key">
                                            &#8674; {{ item.name }}
                                        </div>
                                    </div>
                                </div>
                                <div class="doptions">
                                    <button @click="cancelAddCareer">Cancel</button>
                                </div>
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
    name: 'trio_career',
    props: [
        'info',
    ],
    data(){
        return {
            withCareer: false,
            showBackerList: false,
            findcb: '',
            careerbackers: [
                {
                    name: 'career 1',
                    id: 1
                },
                {
                    name: 'career 2',
                    id: 1
                },
                {
                    name: 'career 3',
                    id: 1
                },
                {
                    name: 'career 4',
                    id: 1
                },
                {
                    name: 'player',
                    id: 1
                },
                {
                    name: 'makesound',
                    id: 1
                },
                {
                    name: 'career 1',
                    id: 1
                },
                {
                    name: 'career 1',
                    id: 1
                },
                {
                    name: 'career 1',
                    id: 1
                },
                {
                    name: 'career 1',
                    id: 1
                },
            ],
            basedcareerbackers: [
                {
                    name: 'career 1',
                    id: 1
                },
                {
                    name: 'career 2',
                    id: 1
                },
                {
                    name: 'career 3',
                    id: 1
                },
                {
                    name: 'career 4',
                    id: 1
                },
                {
                    name: 'player',
                    id: 1
                },
                {
                    name: 'makesound',
                    id: 1
                },
                {
                    name: 'career 1',
                    id: 1
                },
                {
                    name: 'career 1',
                    id: 1
                },
                {
                    name: 'career 1',
                    id: 1
                },
                {
                    name: 'career 1',
                    id: 1
                },
            ]
        }
    },
    methods: {
        addCareer(){
            this.showBackerList = true
        },
        cancelAddCareer(){
            this.showBackerList = false
        },
        findcbnow(){
            console.log('newname ->', this.findcb);
            let smbt = this.basedcareerbackers;

            if(this.findcb == ''){
                this.clientList = smbt;
            } else {
                let spartlist = [];
                smbt.forEach((value, index) => {
                
                    let svalues = value.name;
                    if(svalues.includes(this.findcb)){
                        // console.log('xitem ->', svalues);
                        spartlist.push(value);
                    }
                });

                this.careerbackers = spartlist;
            }

            

            // console.log('zinfo ->', smbt);
        }
    }
}
</script>

<style scoped>
    .clear {
        clear: both;
    }
    .dtio-item {
        background: #fff url('~/assets/images/gold.png') no-repeat center left;
        border-radius: 10px;
        box-shadow: 0px 0px 12px rgb(78 101 167 / 45%);
        overflow: visible;
        position: relative;
        margin-bottom: 20px;
        padding: 15px 20px;
    }
    .trprofinner img {
        width: 100%;
        border-radius: 200px;
    }
    .dnname {
        text-align: center;
        font-size: 21px;
        line-height: 1em;
        margin-bottom: 10px;
    }
    .dfname,
    .demail,
    .dcontact {
        font-size: 13px;
        line-height: 1em;
        margin-bottom: 5px;
    }

    .dbutton .dleftitem,
    .dbutton .drightitem {
        display: inline-block;
        /* width: 50%; */
    }
    .dbutton {
        margin-top: 10px;
    }
    .dbutton button {
        background: #4c63a3;
        background: linear-gradient(137deg, #4c63a3 0%, #364474 100%);
        color: #fff;
        font-size: 11px;
        line-height: 1em;
        padding: 10px 13px;
        border-radius: 15px;
    }
    .dfrom {
        background: #4c63a3;
        background: linear-gradient(137deg, #4c63a3 0%, #364474 100%);
        position: absolute;
        right: 0px;
        top: 7px;
        padding: 10px 30px;
        font-size: 12px;
        line-height: 1em;
        color: #fff;
        border-radius: 5px 0 0 5px;
    }
    .dimage {
        float: left;
        width: 29%;
    }
    .tinfo {
        float: right;
        width: 67%;
        margin-left: 3%;
        margin-top: 30px;
    }


    .pairme {
        font-size: 18px;
        line-height: 1em;
        margin: 25px 0px;
        width: 100%;
        text-align: left;
    }
    .pairme > span {
        border: 2px solid #4c5f99;
        font-size: 40px;
        vertical-align: middle;
        font-weight: bold;
        display: inline-block;
        padding: 0px 7px;
        line-height: 1em;
        border-radius: 100px;
        margin-right: 10px;
        margin-left: 10px;
    }
    .searchpart input {
        width: 80%;
        border: 2px solid #4c5f99;
        margin-bottom: 10px;
        border-radius: 5px;
        line-height: 1em;
        padding: 5px 10px;
    }
    .client_list .cl-item {
        border-bottom: 1px solid #cbd1e6;
        line-height: 1em;
        padding: 7px 10px;
        cursor: pointer;
        text-transform: capitalize;
        transition: all 1s ease-out;
        background: none;
    }
    .client_list .cl-item:hover {
        background: #4c5f99;
        color: #fff;
        transition: all 1s ease-out;
    }
    .client_list {
        width: 80%;
        margin-bottom: 10px;
    }
    .doptions {
        text-align: right;
    }
    .doptions button {
        background: #4c5f99;
        color: #fff;
        line-height: 1em;
        font-size: 12px;
        padding: 10px 20px;
        border-radius: 50px;
        margin-right: 60px;
    }
    .add-client {
        margin-top: 35px;
    }
</style>