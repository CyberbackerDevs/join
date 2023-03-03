<template>
    <div class="setting_positions">
        <div class="dsetleft">
            <div class="doptions">
                <div class="daddbutton" v-if="!showAddPos">
                    <button v-on:click="tooglePos"><span><font-awesome-icon icon="plus-circle" /></span> Add Position</button>
                </div>
                <div class="dshowaddpos" v-if="showAddPos">
                    <h2>Add Position</h2>
                    <div class="dinputpart">
                        <input type="text" v-model="savedPos">
                    </div>
                    <div class="derrorpart" v-if="showErrors != ''">
                        {{showErrors}}
                    </div>
                    <div class="dopts">
                        <button class="cancelbtn" v-on:click="tooglePos">Cancel</button>
                        <button class="savebtn" v-on:click="saveAddPos">Save</button>
                    </div>
                </div>
            </div>
            <div class="showposlist">
                <div :class="'ds-item '+(item.dpstatus == 'active' ? 'active-item' : '')+(item.dpstatus == 'paractive' ? 'paractive-item' : '')" v-for="(item, index) in positions" :key="index">
                    <div class="dtitle">{{item.dpposition}}</div>
                    <div class="dops">
                        <div class="setactive"><v-switch @change="changeOpts($event, item.dpposition, index)" :input-value="(item.dpstatus == 'active' ? true : false)" dense></v-switch></div>
                        <div class="setdelete" v-on:click="removePos(item.id)" v-if="item.dpstatus == 'inactive'"><font-awesome-icon icon="trash-alt" /></div>
                    </div>
                </div>
            </div>
        </div>
        <div class="dsetright">
            <div class="dprevpart">
                <div class=dbasetitle>Position That You're Applying For*</div>
                <div class="dbasesub">Please select the positions you wish to apply</div>
                <div class="listofactivepos">
                    <div class="dlactivepos" v-for="(aitem, aindex) in activePos" :key="aindex">
                        <div class="dcheckbox">
                            <input type="checkbox" name="" id="">
                        </div>
                        <div class="dlabel">{{aitem}}</div>
                    </div>
                </div>
            </div>
            <div class="drops">
                <button v-on:click="saveActivePos"><span><font-awesome-icon icon="save" /></span> Save Positions</button>
            </div>
        </div>
    </div>
</template>

<script>
import { library } from '@fortawesome/fontawesome-svg-core'
import { faTrashAlt, faPlusCircle, faSave } from '@fortawesome/free-solid-svg-icons'
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'

library.add(faTrashAlt, faPlusCircle, faSave)

export default {
    name: 'positions',
    components: {
        FontAwesomeIcon
    },
    data(){
        return {
            showAddPos: false,
            positions: [],
            savedPos: '',
            showErrors: '',
            activePos: [],
            apiHeader: {
                "Access-Control-Allow-Origin": "*",
                "Access-Control-Allow-Methods": "GET, POST, PATCH, PUT, DELETE, OPTIONS",
                "Access-Control-Allow-Headers": "Origin, Content-Type, X-Auth-Token"
            }
        }
    },
    methods: {
        resetAdd(){
            this.showAddPos = false;
            this.showErrors = '';
            this.savedPos = '';
            this.activePos = [];
        },
        saveAddPos(){
            console.log('add Pos ->', this.savedPos);

            if(this.savedPos == ''){
                this.showErrors = 'Please add a position';
                return;
            }
            
            let self = this;

            let params = {
                "position": this.savedPos
            }

            this.$axios.post('https://be2.applytocyberbacker.com/api/admin/settings/positions/add', params, { headers: this.apiHeader })
            .then(function (response) {
                console.log('info -> ', response.data.data);
                
                if(Object.keys(response.data.data).length === 0){
                    self.showErrors = response.data.message;
                } 
                self.resetAdd();
                self.getPositions();
                
                
            })
            .catch(function (error) {
                console.log(error);
            });
        },
        saveActivePos(){
            console.log('add Active Pos ->', this.activePos);

            let self = this;

            let params = {
                "position": this.activePos
            }

            this.$axios.post('https://be2.applytocyberbacker.com/api/admin/settings/positions/available/save', params, { headers: this.apiHeader })
            .then(function (response) {
                console.log('active pots -> ', response.data);
                
                self.resetAdd();
                self.getPositions();
                
                
            })
            .catch(function (error) {
                console.log(error);
            });


        },
        tooglePos(){
            this.showAddPos = !this.showAddPos;
        },
        removePos(selected){
            console.log('remove ->', selected);

            let self = this;

            let params = {
                "position": selected
            }

            // this.$axios.post('https://be2.applytocyberbacker.com/api/admin/settings/positions/delete', params, { headers: this.apiHeader })
            // .then(function (response) {
            //     console.log('active pots -> ', response.data);
                
            //     self.resetAdd();
            //     self.getPositions();
                
                
            // })
            // .catch(function (error) {
            //     console.log(error);
            // });
        },
        changeOpts(event, selected, index){
            console.log('event ->', event);
            console.log('selected ->', selected);
               
            if(event){
                this.activePos.push(selected);
                this.positions[index].dpstatus = 'paractive'; 
            } else {
                this.positions[index].dpstatus = 'inactive'; 
                let actpos = this.activePos;
                let removeitem = actpos.indexOf(selected);
                
                if (removeitem > -1) {
                    this.activePos.splice(removeitem, 1);
                }
            }
        },
        getPositions(){
            let self = this;

            this.$axios.get('https://be2.applytocyberbacker.com/api/admin/settings/positions/list', {}, { headers: this.apiHeader })
            .then(function (response) {
                console.log('info -> ', response.data.data.positions);
                self.positions = response.data.data.positions;
                response.data.data.positions.map(function(value, key) {
                    if(value.dpstatus == 'active'){
                        self.activePos.push(value.dpposition);
                    }
                });
                
            })
            .catch(function (error) {
                console.log(error);
            });
        }
    },
    mounted(){
        this.getPositions();
    }
}
</script>

<style scoped>
    .setting_positions {
        background: #fff;
        border-radius: 5px;
        padding: 20px;
    }
    .dpagetitle {
        font-size: 28px;
        font-weight: 600;
        line-height: 1em;
        margin-bottom: 10px;
    }
    .setting_positions .dsetleft {
        display: inline-block;
        width: 59%;
        vertical-align: top;
    }
    .setting_positions .dsetright {
        display: inline-block;
        width: 40%;
    }

    .showposlist .ds-item {
        display: inline-block;
        width: 48%;
        margin-right: 2%;
        border: 1px solid #4c5f99;
        border-radius: 5px;
        padding: 5px 20px;
        margin-bottom: 2%;
    }
    .showposlist .ds-item.paractive-item {
        border: 1px solid #f7fc00;
        background: #f7fc0060;
    }
    .showposlist .ds-item.active-item {
        border: 1px solid #52ff35;
        background: #52ff3560;
    }

    .showposlist .ds-item .dtitle {
        display: inline-block;
        width: 68%;
        vertical-align: middle;
    }
    .showposlist .ds-item .dops {
        display: inline-block;
        width: 30%;
        vertical-align: middle;
        text-align: right;
    }
 
    .showposlist .ds-item .dops > * {
        display: inline-block;
    }
    
    .showposlist .ds-item .dops .setdelete {
        background: #4c5f99;
        padding: 5px 9px;
        border-radius: 20px;
        color: #fff;
        font-size: 11px;
        cursor: pointer;
    }

    .showposlist .ds-item .dtitle {
        font-weight: bold;
        line-height: 1em;
    }

    .dshowaddpos {
        width: 94%;
        margin-bottom: 30px;
        box-shadow: 0 0 12px #7e88a660;
        border-radius: 10px;
        padding: 15px 20px;
    }

    .dshowaddpos h2 {
        font-size: 16px;
        line-height: 1em;
        margin-bottom: 10px;
    }

    .dshowaddpos .dinputpart input {
        width: 100%;
        border: 1px solid #4c5f99;
        border-radius: 5px;
        margin-bottom: 10px;
        padding: 10px 15px;
    }
    .dshowaddpos .dopts {
        text-align: right;
    }

    .dshowaddpos .dopts button{
        color: #fff;
        font-size: 12px;
        line-height: 1em;
        padding: 10px 15px;
        border-radius: 50px;
        text-transform: uppercase;
        letter-spacing: 1px;
    }
    .dshowaddpos .dopts button.cancelbtn {
        background: #4d64a7;
        background: linear-gradient(137deg, #485b96 0%, #4d64a7 100%);
    }

    .dshowaddpos .dopts button.savebtn {
        background: #fe608b;
        background: linear-gradient(137deg, #fe608b 0%, #ff99a2 100%);
    }

    .daddbutton button,
    .dsetright .drops button {
        background: #fe608b;
        background: linear-gradient(137deg, #fe608b 0%, #ff99a2 100%);
        color: #fff;
        font-size: 12px;
        line-height: 1em;
        padding: 10px 15px;
        border-radius: 50px;
        text-transform: uppercase;
        letter-spacing: 1px;
        display: inline-block;
    }

    .daddbutton button span {
        margin-right: 5px;
    }

    .doptions {
        margin-bottom: 15px;
    }

    .derrorpart {
        color: red;
        font-size: 12px;
    }

    .dsetright .drops {
        text-align: right;
    }

    .listofactivepos .dlactivepos {
        display: inline-block;
        width: 49%;
    }

    .listofactivepos .dlactivepos .dcheckbox {
        display: inline-block;
        width: 10%;
        vertical-align: middle;
    }

    .listofactivepos .dlactivepos .dlabel {
        display: inline-block;
        width: 88%;
        vertical-align: middle;
        font-weight: bold;
        margin-bottom: 5px;
    }

    .dprevpart {
        width: 90%;
        margin-left: 9%;
        border: 1px solid #4c5f99;
        padding: 20px;
        border-radius: 5px;
        margin-bottom: 25px;
        margin-top: 45px;
    }

    .dprevpart .dbasetitle {
        font-size: 18px;
        font-weight: bold;
        line-height: 1em;
        margin-bottom: 3px;
    }

    .dprevpart .dbasesub {
        font-size: 12px;
        margin-bottom: 15px;
    }

</style>

<style>
    .showposlist .ds-item .dops .setactive .v-input {
        margin: 0;
        padding: 0;
    }

    .showposlist .ds-item .dops .setactive .v-input .v-messages.theme--light {
        display: none;
    }

    .showposlist .ds-item .dops .setactive .v-input .v-input__slot {
        margin: 0;
    }
</style>