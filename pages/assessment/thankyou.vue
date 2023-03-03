<template>
    <div class="step_one">
        <v-app fluid>
            <div class="dthankyoupage">
                <div class="dloadingpart" v-if="showLoading">
                    <div class="dlp-inner">
                        <v-progress-circular
                            :size="70"
                            indeterminate
                            color="primary"
                        ></v-progress-circular>
                    </div>
                </div>
                <!-- <h3>Thank you for Applying</h3> -->
                <div v-if="!showLoading">
                    <div class="interview_later_form" v-if="showInterviewLaterForm">
                        <div class="dinfobase">
                            <div class="di-inner">
                                <h2>Do interview later</h2>
                                <div class="dsubshowmessage">We would really want you on board </div>
                                <div class="drequirements">
                                    <div class="drq-requirements interview_later">
                                        <h2>Please specify a reason why you cannot<br />join the interview</h2>
                                        <div class="interview_base">
                                            <select v-model="selectedAnswer">
                                                <option value="">Please select one</option>
                                                <option :value="item" v-for="(item, index) in laterOptions" :key="index">{{item}}</option>
                                            </select>
                                            <div class="form_error" v-if="activeError">Please select a reason why you cannot join the interview</div>
                                        </div>
                                    </div>
                                </div>
                                <div class="d-submit-form">
                                    <button v-if="!surveyLoading" class="submit-button" v-on:click="submitSurvey">Submit survey</button>
                                     <v-progress-circular
                                        v-if="surveyLoading"
                                        :size="25"
                                        indeterminate
                                        color="primary"
                                    ></v-progress-circular>  
                                </div>
                            </div>

                        </div>
                    </div>
                    <div class="show_thanks" v-if="!showInterviewLaterForm">
                        <div class="dinfobase" v-if="isPassed">
                            <div class="di-inner">
                                <h2>Thank you!</h2>
                                <div class="dshowmessage">You have completed the application form.</div>
                                <div class="dsubshowmessage">The next step is to have your initial interview using your home computer via zoom. The interview will be conducted on a first-come, first served basis.</div>
                                <div class="drequirements">
                                    <div class="drq-requirements">
                                        <h2>Please make sure you have the following</h2>
                                        <ul>
                                            <li>Webcam turned on and available</li>
                                            <li>Headset plugged in and ready to go</li>
                                            <li>On a computer and not a mobile device!</li>
                                            <li>Stable internet connection you would be using for work</li>
                                            <li>In work station that will be used during working hours</li>
                                        </ul>
                                    </div>
                                </div>
                                <div class="dreminder">We are open 24/7 so you can come in anytime you want without setting an appointment. Please expect a waiting time due to the huge volume of applicants.</div>
                                <div class="dreminder">Hope to hear from you soon!</div>
                                <div class="doptions">
                                    <!-- <button @click="goBackHome">Do interview later</button> -->
                                    <button @click="ProceedWithInitialInterview">Proceed with the initial interview</button>
                                </div>
                            </div>
                        </div>
                        <div class="dinfobase" v-if="!isPassed">
                            <div class="di-inner">
                                <h2>Thank you for applying!</h2>
                                <div class="dshowmessage">We greatly appreciate your interest in working with Cyberbacker. We regret to inform you that the result of your Values Assessment does not match any of the job positions we are currently hiring for.</div>
                                <br>
                                <div class="dshowmessage">You may reapply to Cyberbacker after 60 days from the receipt of this email. We hope by then we may have a role that will be a best fit for you. </div>
                                <br>
                                <div class="dshowmessage">Please note that not all Cyberbackers made it on their first try.</div>
                                <br>
                                <div class="dshowmessage">We wish you the best in your job search and good luck!</div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </v-app>
    </div>
</template>

<script>
    import draggable from 'vuedraggable'
    import { print } from 'graphql';
    import gql from 'graphql-tag';

    export default {
        head: {
            title: 'Thank you',
        },
        name: 'step_three',
        layout: 'StageOne',
        components: {
            draggable,
        },
        data(){
            return {
                isPassed: true,
                showLoading: true,
                ApplicantStatus: '',
                showInterviewLaterForm: false,
                selectedAnswer: '',
                userid: '',
                surveyLoading: false,
                activeError: false,
                laterOptions: [
                    "I am not in my computer.",
                    "I don't have a computer.",
                    "I don't have a headset or webcam.",
                    "I am not currently at home.",
                    "I am currently employed.",
                    "I am not prepared to have my initial interview today.",
                ]
            }
        },
        methods: {
            goBackHome(){
                console.log('Do interview later');
                this.showInterviewLaterForm = true;
                // window.location.href = 'https://cyberbacker.com/'\

            },
            ProceedWithInitialInterview(){
                console.log('go to initial interview');
                window.location.href = 'https://cyberbacker.zoom.us/j/977857097'
            },
            checkStatus(){
                let userinfo = this.$route.query.uuid;
                let self = this;

                const headers = {
                    'Access-Control-Allow-Origin': '*',
                    'token': userinfo
                }

                console.log(headers);

                this.$axios.get('https://be2.applytocyberbacker.com/api/user/info', { headers: headers}, {})
                .then(function (response) {
                    console.log('responses -> ', response.data.data.email.status);
                    self.ApplicantStatus = response.data.data.email.status;
                    if(self.ApplicantStatus == 'YAssessmentDone'){
                        self.isPassed = true;
                    }
                    if(self.ApplicantStatus == 'InitialInterview'){
                        self.isPassed = true;
                    }
                    self.showLoading = false;
                })
                .catch(function (error) {
                    console.log('show error ->', error);
                });
            },
            submitSurvey(){
                console.log('selected answer -> ', this.selectedAnswer);
                

                if(this.selectedAnswer == ''){
                    this.activeError = true;
                    return;
                }

                this.surveyLoading = true;
                console.log('no error');

                let self = this;

                const headers = {
                    'Access-Control-Allow-Origin': '*',
                    'token': this.userid
                }

                let params = {
                    "token": this.userid,
                    "reason": this.selectedAnswer
                }
                

                this.$axios.post('https://be2.applytocyberbacker.com/api/interview/later', params, { headers: headers })
                .then(function (response) {
                    console.log('saved interview later ->', response);
                        window.location.replace("https://cyberbacker.com/");
                }).catch(function (error) {
                    console.log('show error ->', error);
                });

            },
            setupUserID(){
                this.userid = this.$route.query.uuid;
            },
        },
        mounted() {
            this.checkStatus();
            this.setupUserID();
        }
    }
</script>
<style>
    .step_one .v-application--wrap{
        min-height: 60vh;
    }
</style>
<style scoped>
   .dloadingpart {
        text-align: center;
        margin-top: 20vh;
    }
    .step_one{
        background: #edf8fc;
        padding: 50px 60px;
        border-radius: 10px;
        opacity: .9;
        font-family: 'Poppins' !important;
    }
    .step_one > .theme--light {
        background: none !important;
    }
    .dthankyoupage > h3 {
        text-align: center;
        line-height: 1em;
        font-size: 35px;
        font-weight: 400;
        margin-bottom: 40px;
    }
    .dthankyoupage .di-inner > h2 {
        text-align: center;
        line-height: 1em;
        font-size: 71px;
        font-weight: 100;
        margin-bottom: 12px;
        text-transform: uppercase;
    }
    .dthankyoupage .di-inner > .dshowmessage,
    .dthankyoupage .di-inner > .dreminder,
    .dthankyoupage .di-inner > .dsubshowmessage {
        text-align: center;
        font-size: 21px;
        line-height: 1.2em;
        margin-bottom: 0px;
    }
    .drequirements {
        width: 65%;
        margin: 25px auto;
    }
    .drq-requirements {
        border: 2px solid #000;
    }
    .drq-requirements .interview_base {
        padding: 20px;
    }
    .drq-requirements select {
        width: 100%;
        display: block;
        border: 2px solid #aaa;
        padding: 10px;
        border-radius: 10px;
    }
    .drq-requirements > h2 {
        text-align: center;
        line-height: 1em;
        font-size: 26px;
        text-transform: uppercase;
        background: #000;
        color: #fff;
        font-weight: 100;
        padding: 15px 0;
    }
    .drq-requirements ul {
        padding: 25px 55px;
    }
    .doptions {
        text-align: center;
        margin-top: 26px;
    }
    .doptions button {
        background: #007dcf;
        background: linear-gradient(95deg, #007dcf 0%, #5191ad 100%);
        line-height: 1em;
        color: #fff;
        padding: 15px 30px;
        font-weight: 100;
        font-size: 18px;
        border-radius: 45px;
        margin: 0 7px;
    }
    button.submit-button {
        text-align: center;
        margin: 0 auto;
        display: block;
        background: #3599cd;
        color: #fff;
        font-size: 16px;
        text-transform: uppercase;
        line-height: 1em;
        padding: 15px 70px;
        border-radius: 5px;
    }
    .d-submit-form {
        text-align: center;
    }
    .form_error {
        color: red;
        font-size: 12px;
        line-height: 1em;
        margin-top: 5px;
    }
</style>