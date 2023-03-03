<template>
    <div class="general-settings">
        <div class="d-people-list">
            <div class="d-people-inner">
                <div class="career-options daddbutton" v-if="!showAddPeople">
                    <button v-on:click="tooglePos"><span><font-awesome-icon icon="plus-circle" /></span> Add People</button>
                </div>
                <div class="career-add-people" v-if="showAddPeople">
                    <h3>Add People</h3>
                    <div class="add-people-form">
                        <div class="career-email">
                            <input type="text" v-model="careerEmail" name="" id="" placeholder="Enter email address">
                            <div class="d-error-message">{{errorMessage}}</div>
                        </div>
                    </div>
                    <div class="career-add-people-options">
                        <button class="d-cancel-btn" v-on:click="closeAddPeople">Cancel</button>
                        <button class="d-save-btn" v-on:click="saveCareerPeople">Save</button>
                    </div>
                </div>
                <div class="career-people">
                    <v-card>
                        <v-card-title>
                            <v-text-field v-model="search" append-icon="mdi-magnify" label="Search" single-line hide-details
                            ></v-text-field>
                        </v-card-title>
                        <v-data-table :headers="headers" :items="careerPeople" :items-per-page="15" :search="search" class="elevation-1" >
                            <template v-slot:item.actions="{ item }">
                                <!-- <v-icon small class="mr-2" @click="editItem(item)" >mdi-pencil</v-icon> -->
                                <v-icon small @click="deleteItem(item)" >mdi-delete</v-icon>
                            </template>
                        </v-data-table>
                    </v-card>
                </div>
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
    name: 'people',
    components: {
        FontAwesomeIcon
    },
    data(){
        return {
            showAddPeople: false,
            careerEmail: '',
            errorMessage: '',
            search: '',
            headers: [
                { text: 'Name', value: 'name' },
                { text: 'Email', value: 'email' },
                { text: 'Actions', value: 'actions', sortable: false },
            ],
            careerPeople: []
        }
    },
    methods: {
        closeAddPeople(){
            this.showAddPeople = false;
        },
        tooglePos(){
            this.showAddPeople = true;
        },
        deleteItem(selectedItem){
            console.log('this -> ', selectedItem);

            let header = {
                "Access-Control-Allow-Origin": "*",
                "Access-Control-Allow-Methods": "GET, POST, PATCH, PUT, DELETE, OPTIONS",
                "Access-Control-Allow-Headers": "Origin, Content-Type, X-Auth-Token"
            }

            let params = {
                email: selectedItem.email
            }

            let self = this;

            this.$axios.post('https://be2.applytocyberbacker.com/api/users/career/remove', params, { headers: header })
            .then(function (response) {

                console.log(response.data);

                self.careerPeople = [];
                self.getAllCareerPeople();
            })
            .catch(function (error) {
                console.log(error);
            });

        },
        saveCareerPeople(){
            this.errorMessage = '';
            console.log('this is atest');

            let caremail = this.careerEmail;

            if(caremail== ''){
                this.errorMessage = 'Field must not be empty';
                return false;
            }

            if(caremail.indexOf("cyberbacker.com") <= 0){
                this.errorMessage = 'User must be a cyberbacker';
                return false;
            }

            let header = {
                "Access-Control-Allow-Origin": "*",
                "Access-Control-Allow-Methods": "GET, POST, PATCH, PUT, DELETE, OPTIONS",
                "Access-Control-Allow-Headers": "Origin, Content-Type, X-Auth-Token"
            }

            let params = {
                email: caremail
            }

            let self = this;

            this.$axios.post('https://be2.applytocyberbacker.com/api/users/career/add', params, { headers: header })
            .then(function (response) {

                console.log(response.data);
                self.showAddPeople = false;
                self.careerPeople = [];
                self.getAllCareerPeople();
            })
            .catch(function (error) {
                console.log(error);
            });
            
            console.log('save new carrerr personel');
            



        },
        getAllCareerPeople(){
            console.log('get all career in settings');

            // get all careers
            let self = this;

            let header = {
                "Access-Control-Allow-Origin": "*",
                "Access-Control-Allow-Methods": "GET, POST, PATCH, PUT, DELETE, OPTIONS",
                "Access-Control-Allow-Headers": "Origin, Content-Type, X-Auth-Token"
            }
            let params = {}
            
            this.$axios.post('https://be2.applytocyberbacker.com/api/users/career/all', params, { headers: header })
            .then(function (response) {

                self.careerPeople = response.data.data;
            })
            .catch(function (error) {
                console.log(error);
            });
            
        }
    },
    mounted(){
        this.getAllCareerPeople();
    }
}
</script>

<style scoped>
    .career-options {
        margin-bottom: 15px;
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
    .career-add-people {
        background: #fff;
        margin-bottom: 15px;
        border-radius: 5px;
        padding: 13px 20px;
    }
    .add-people-form input[type="text"] {
        border: 1px solid #4d5e99;
        padding: 5px 10px;
        width: 75%;
        border-radius: 5px;
        margin-bottom: 10px;
        margin-top: 10px;
    }

    button.d-cancel-btn {
        background: #fa7b7b;
        color: #fff;
        font-size: 13px;
        line-height: 1em;
        padding: 10px 15px;
        border-radius: 50px;
        font-weight: bold;
        margin-right: 5px;
    }

    button.d-save-btn {
        background: #65b874;
        color: #fff;
        font-size: 13px;
        line-height: 1em;
        padding: 10px 15px;
        border-radius: 50px;
        font-weight: bold;
    }
</style>