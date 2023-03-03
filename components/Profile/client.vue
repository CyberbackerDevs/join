<template>
    <div class="dclient">
        <div class="dclient-inner">
            <div class="dcontent">
                <div class="dclient-info-inner">
                    <div class="info-container" v-if="hasClient">
                        <div class="dclientprof">
                            <div class="cprofinner">
                                <img src="~/assets/images/prof_icon.png" />
                            </div>
                        </div>
                        <div class="ddetails">
                            <h3>{{info.name}}</h3>
                            <div class="cemail">{{info.email}}</div>
                            <div class="cphone">{{info.phone}}</div>
                        </div>
                        <br class="clear">
                        <div class="dleavemessage">
                            <NuxtLink to="/growth/client">View Client Profile</NuxtLink>
                            <!-- <button>View Client Profile</button> -->
                        </div>
                    </div>
                    <div class="info-container" v-if="!hasClient">
                        <div class="dic-inner" v-if="!showClient">
                            <button class="pairme" @click="addClient"><span>+</span> Pair with client</button>
                        </div>
                        <div class="add-client" v-if="showClient">
                            <div class="ac-client-list">
                                <div class="searchpart">
                                    <input type="text" v-model="findcb" name="searchc" placeholder="Find Client" v-on:keyup="findcbnow">
                                </div>
                                <div class="client_list">
                                    <div class="cl_inner">
                                        <div class="cl-item" v-for="(item, key) in clientList.slice(0, 10)" :key="key">
                                            &#8674; {{ item.name }}
                                        </div>
                                    </div>
                                </div>
                                <div class="doptions">
                                    <button @click="cancelAddClient">Cancel</button>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <div class="dimae">
                <img src="~/assets/images/man_white.png" />
            </div>
        </div>
    </div>
</template>

<script>
export default {
    head: {
        title: 'Profile',
    },
    name: 'client',
    props: [
        'info',
    ],
    data(){
        return {
            hasClient: false,
            showClient: false,
            findcb: '',
            clientList: [
                {
                    name:"client 1",
                    id: 1
                },
                {
                    name:"client 2",
                    id: 1
                },
                {
                    name:"client 3",
                    id: 1
                },
                {
                    name:"client 4",
                    id: 1
                },
                {
                    name:"client 5",
                    id: 1
                },{
                    name:"client 6",
                    id: 1
                },
                {
                    name:"superman",
                    id: 1
                },
                {
                    name:"batman",
                    id: 1
                },
                {
                    name:"party",
                    id: 1
                },
                {
                    name:"client 1",
                    id: 1
                },
                {
                    name:"client 1",
                    id: 1
                },
                {
                    name:"client 1",
                    id: 1
                },{
                    name:"client 1",
                    id: 1
                },
                {
                    name:"client 1",
                    id: 1
                }
            ],
            batchclientList: [
                {
                    name:"client 1",
                    id: 1
                },
                {
                    name:"client 2",
                    id: 1
                },
                {
                    name:"client 3",
                    id: 1
                },
                {
                    name:"client 4",
                    id: 1
                },
                {
                    name:"client 5",
                    id: 1
                },{
                    name:"client 6",
                    id: 1
                },
                {
                    name:"superman",
                    id: 1
                },
                {
                    name:"batman",
                    id: 1
                },
                {
                    name:"party",
                    id: 1
                },
                {
                    name:"client 1",
                    id: 1
                },
                {
                    name:"client 1",
                    id: 1
                },
                {
                    name:"client 1",
                    id: 1
                },{
                    name:"client 1",
                    id: 1
                },
                {
                    name:"client 1",
                    id: 1
                }
            ]
        }
    },
    methods: {
        addClient(){
            this.showClient = true
        },
        cancelAddClient(){
            this.showClient = false
        },
        findcbnow(){
            console.log('newname ->', this.findcb);
            let smbt = this.batchclientList;

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

                this.clientList = spartlist;
            }

            

            // console.log('zinfo ->', smbt);
        }
    },
    mounted(){

    }
}
</script>

<style scoped>
    .clear {
        clear: both;
    }
    .dclient-inner {
        background: #fff url('~/assets/images/gold.png') no-repeat center left;
        border-radius: 10px;
        box-shadow: 0px 0px 12px rgb(78 101 167 / 45%);
        overflow: visible;
        position: relative;
    }
    .dclient-inner .dimae {
        position: absolute;
        bottom: 0;
        right: -20px;
    }
    .dclient-inner .dimae img {
        width: 95px;
        vertical-align: bottom;
    }
    .dclient-info-inner {
        padding: 15px 20px;
    }
    .dclient-info-inner h3 {
        font-size: 16px;
        font-weight: 400;
        line-height: 1em;
        margin-bottom: 5px;
    }
    .dclient-info-inner .cemail {
        font-size: 12px;
        line-height: 1em;
        margin-bottom: 5px;
    }
    .dclient-info-inner .cphone {
        font-size: 12px;
        line-height: 1em;
        margin-bottom: 5px;
    }
    .dclient {
        margin-bottom: 20px;
    }
    .dclientprof {
        width: 60px;
        margin-bottom: 5px;
        float: left;
    }
    .dclientprof img {
        width: 100%;
        border-radius: 100px;
    }
    .ddetails {
        float: left;
        margin-left: 10px;
    }
    .dleavemessage button,
    .dleavemessage a {
        background: #4c63a3;
        background: linear-gradient(137deg, #4c63a3 0%, #364474 100%);
        color: #fff;
        padding: 10px 15px;
        line-height: 1em;
        border-radius: 50px;
        display: inline-block;
        text-decoration: none;
        /* margin-top: 10px; */
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
</style>