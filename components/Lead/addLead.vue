<template>
    <div class="dselectedinner">
        <div class="dheadselectedinfo" :style="{ background: 'url(' + require('@/assets/images/placeheader.jpg') + ') center right no-repeat' }">
            <div class="sheadinner">
                <div class="dtitle">
                    <h2>Add Source Leads</h2>
                    <div class="subhead">add new source leads</div>
                </div>
                <div class="dsitems sendinvite">
                    <div class="dprogress" v-if="showLoading">
                        <v-progress-circular
                            :size="25"
                            indeterminate
                            color="amber"
                        ></v-progress-circular>    
                    </div>
                    <div class="dbutton" v-if="!showLoading">
                        <PageAddButton v-on:click.native="addLeadNow" label="Save Source Lead"/>
                    </div>
                </div>
            </div>
        </div>
        <div class="reply_based">
            <div class="dformhere">
                <div class="form-item">
                    <label for="">Firstname</label>
                    <input type="text" name="name" v-model="addSourceLead.firstname">
                    <!-- <small>file must be either csv or excell format</small> -->
                </div>
                <div class="form-item">
                    <label for="">Lastname</label>
                    <input type="text" name="email" v-model="addSourceLead.lastname">
                    <!-- <small>file must be either csv or excell format</small> -->
                </div>
                <div class="form-item">
                    <label for="">Phone number</label>
                    <input type="text" name="phone" v-model="addSourceLead.phone">
                    <!-- <small>(000) 000-0000</small> -->
                </div>
                <div class="form-item">
                    <label for="">Email Address</label>
                    <input type="text" name="email" v-model="addSourceLead.email">
                    <!-- <small>file must be either csv or excell format</small> -->
                </div>
                <div class="form-item">
                    <v-menu
                        v-model="showPicker"
                        :close-on-content-click="false"
                        transition="scale-transition"
                        max-width="290px"
                        min-width="290px"
                    >
                        <template v-slot:activator="{ on }">
                            <label for="">Birthdate</label>
                            <input type="text" v-model="addSourceLead.date" persistent-hint readonly v-on="on">
                            <!-- <v-text-field
                                v-model="selectedDate"
                                label="Birthdate"
                                persistent-hint
                                readonly
                                v-on="on"
                            ></v-text-field> -->
                        </template>
                        <v-date-picker
                            v-model="addSourceLead.date"
                            no-title
                            @input="showPicker = false"
                            :show-current="true"
                        ></v-date-picker>
                    </v-menu>
                    <!-- <label for="">Birthdate</label>
                    <input type="text" name="date" v-model="addSourceLead.date">
                    <small>when the data is scraped</small> -->
                </div>
                <div class="form-item">
                    <label for="">Origin Source</label>
                    <input type="text" name="ogirin" v-model="addSourceLead.origin">
                    <small>Jobstreet, TikTok, Facebook, etc.</small>
                </div>
                
                <div class="form-item full-width">
                    <label for="">Bio</label>
                    <textarea name="reply" v-model="addSourceLead.bio"></textarea>
                    <!-- <small>If the lead responsed in the invice</small> -->
                </div>

                <br class="clear">  
            </div>
        </div>
    </div>
</template>

<script>
import { print } from 'graphql';
import gql from 'graphql-tag';

export default {
    name: 'addLead',
    props: [
        'info'
    ],
    data(){
        return {
            showLoading: false,
            showPicker: false,
            addSourceLead: {
                firstname: '',
                lastname: '',
                email: '',
                phone: '',
                date: '',
                origin: '',
                bio: '',
            },
        }
    },
    methods: {
        addLeadNow() {
            this.$emit('clicked', this.addSourceLead)
            this.showLoading = true;

            let self = this;
            const CREATE_ACCOUNT = gql`
                mutation createAccount($firstName: String!, $email: String!, $lastName: String!, $mobile: String!, $birthdate: String!, $bio: String!){
                    createAccount(firstName:$firstName, email:$email, lastName:$lastName, mobile:$mobile, birthdate:$birthdate, bio:$bio){
                        id
                        firstName
                        email
                        lastName
                        mobile
                        birthdate
                        bio
                    }
                }
            `;

            const headers = {
                'Access-Control-Allow-Origin': '*',
            }

            this.$axios.post('https://cyberbacker-ms-api.herokuapp.com/graphql', {
                query: print(CREATE_ACCOUNT),
                variables: {
                    firstName: this.addSourceLead.firstname,
                    email: this.addSourceLead.email,
                    lastName: this.addSourceLead.lastname,
                    mobile: this.addSourceLead.phone,
                    birthdate: this.addSourceLead.date,
                    bio: this.addSourceLead.bio,
                },
            },
            {
                headers: headers
            })
            .then(function (response) {
                console.log('info -> ', response);
                // const linfo = JSON.stringify(response.data.data);
                // localStorage.setItem('applicant_info', linfo);
                
                // console.log(linfo);
                // self.$router.push('/assessment/step2')
                window.location.reload()
            })
            .catch(function (error) {
                console.log(error);
            });

        }
    }
}
</script>

<style scoped>
    .clear {
        clear: both;
    }
    .dselectedinner {
        border-radius: 5px;
        overflow: hidden;
    }
    .dheadselectedinfo {
        background-size: cover !important;
        padding: 30px 30px 15px;
        color: #fff;
    }
    .dtitle {
        display: inline-block;
        width: 60%;
        vertical-align: top;
    }

    .dsitems.sendinvite {
        display: inline-block;
        width: 39%;
        text-align: right;
        vertical-align: middle;
    }
    .dheadselectedinfo h2 {
        font-size: 26px;
        font-weight: 400 !important;
        line-height: 1em;
        margin-bottom: 5px;
    }

    .subhead {
        margin-bottom: 10px;
        line-height: 1em;
        font-size: 14px;
        font-weight: 100;
    }

    .reply_based {
        background: #fff;
        padding: 15px 30px;
        font-size: 14px;
    }

    .form-item {
        float: left;
        width: 50%;
        margin-bottom: 20px;
    }

    .form-item.full-width {
        width: 100%;
        float: none;
    }

    .form-item label {
        display: block;
        font-size: 13px;
        line-height: 1em;
        margin-bottom: 5px;
    }

    .form-item input,
    .form-item textarea {
        display: block;
        width: 95%;
        border-radius: 5px;
        border: 1px solid #4c61a0;
        line-height: 1em;
        padding: 10px 15px;
        font-size: 12px;
    }

    .form-item textarea {
        min-height: 150px;
    }

    .form-item small {
        font-size: 12px;
        font-style: italic;
    }
    
</style>