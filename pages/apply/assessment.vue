<template>
    <v-app>
        <div class="d-start-appliation" >
            <ApplyStepper step="3"/>
            <div class="d-start-header">
                <h1>Y-assessment</h1>
            </div>
            <div class="dformitem">
                <div class="dwelcomeheader" >
                    <div class="subwh">The Y-assessment helps understand the reasons that drive an individual to utilize<br />their talents in the unique way they do. It helps determine their motivators and drivers.</div>
                    <div class="subwh">There will be a different list of items for each question.<br />Please arrange the options that represent your thoughts by clicking and<br />dragging your “<strong>most preferential</strong>” answer to “<strong>least preferential.</strong>”</div>
                    <div class="subwh">It will take you about 15 minutes to complete the assessment.<br />Please find a quiet place where you will not be interrupted before starting.</div>
                </div>
                 <div class="submit-values" v-if="!aftersubmitloading">
                    <button v-on:click="proceedToValues" v-if="!stats">Take Values Assessment</button>
                    <button v-on:click="submit" v-if="stats">Confirm Values Assessment Completed</button>
                 </div>
                 <div class="show-loading-after-submit" v-if="aftersubmitloading">
                    <img :src="require('@/assets/images/loadingme.gif')" alt="">
                </div>
                <!-- <div class="new_list">
                    <div class="dloadingpart" v-if="showQuestionsLoading">
                        <div class="dlp-inner">
                            <v-progress-circular
                                :size="70"
                                indeterminate
                                color="primary"
                            ></v-progress-circular>
                        </div>
                    </div>
                    <div class="newquestions" v-if="!showQuestionsLoading">
                        <v-expansion-panels v-model="panel" >
                            <v-expansion-panel v-for="(item, index) in bquest" :key="item.avalue" :class="(currentQuestions <= index ? '' : 'question-done')" :disabled="(currentQuestions == index ? false : true)" multiple>
                                <v-expansion-panel-header >
                                    <span class="d-question-title">Question {{index + 1}}</span>
                                    <div class="v-expansion-panel-header__icon" :class="(currentQuestions <= index ? 'hide-check' : 'show-check')"><font-awesome-icon icon="check-circle" /></div>
                                </v-expansion-panel-header>
                                <v-expansion-panel-content>
                                    <div class="dquestiontitle">{{ item.avalue }}</div>
                                    <div class="dquestionsuntitle">{{ item.aref }}</div>
                                    <div class="danswers">
                                        <div class="dnote">Most Preferential</div>
                                        <draggable v-model="item.answers" ghost-class="ghost" @end="onEnd">
                                            <transition-group type="transition" name="flip-list">
                                                <div class="sortable" :id="indx" v-for="(itemx, indx) in item.answers" :key="itemx.avalue">
                                                    <strong>{{ itemx.avalue }}</strong>
                                                </div>
                                            </transition-group>
                                        </draggable>
                                        <div class="dnote">Least Preferential</div>
                                    </div>
                                    <div class="dproceedMessage" v-if="nextTextBase !== ''">
                                        <span>{{nextTextBase}}</span> <button v-on:click="confirmProceedtoNextQuestion(index)">Proceed to next question <span><font-awesome-icon icon="arrow-right" /></span></button>
                                    </div>
                                    <div class="dnextquestion" v-if="nextTextBase === ''">
                                        <button class="dnextbtn" v-on:click="proceedToNextQuestion(index)" :class="(currentQuestions == maxIndex ? 'hide-button' : '')">Next Question <span><font-awesome-icon icon="arrow-right" /></span></button>
                                    </div>
                                </v-expansion-panel-content>
                            </v-expansion-panel>
                        </v-expansion-panels>
                    </div>
                </div> -->
            </div>
            <!-- <div class="dsubmitforms" v-if="!showLoading">
                <div class="subs" v-if="hideSubmit">
                    <button type="submit" v-on:click="submit">Submit Y-assessment</button>
                </div>
                <div class="show-loading-after-submit" v-if="aftersubmitloading">
                    <img :src="require('@/assets/images/loadingme.gif')" alt="">
                </div>
            </div> -->
            
        </div>
    </v-app>
</template>

<script>
import draggable from 'vuedraggable'

import { library } from '@fortawesome/fontawesome-svg-core'
import { faCheckCircle} from '@fortawesome/free-regular-svg-icons'
import { faArrowRight, faArrowLeft } from '@fortawesome/free-solid-svg-icons'
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'

library.add(faCheckCircle, faArrowRight, faArrowLeft)

export default {
    head: {
        title: 'Y-Assessment Questions',
    },
    name: 'assessment_final',
    layout: 'Avtwo',
    components: {
        draggable,
        FontAwesomeIcon
    },
    data(){
        return {
            showLoading: true,
            hideSubmit: false,
            aftersubmitloading: false,
            uuid: '',
            stats: false,
            bquest: [], // list of questions
            panel: 0, // what panel is showing in the yassessment form
            currentQuestions: 0, // current question where the applicant is answering
            maxIndex: 0, // get max number of questions
            oldIndex: '', // used for the arrangement of the y-assessment answers
            newIndex: '', // used for the arrangement of the y-assessment answers
            showQuestionsLoading: true,
            nextTextBase: '', // define if the appliant change something in the answers or not
            hasElementReArranged: false,
            answers: {
                individualistic: 0,
                theoretical: 0,
                aesthetic: 0,
                economic: 0,
                regulatory: 0,
                political: 0,
                altruist: 0,
            },
            fillers: [],
        }
    },
    methods: {
        proceedToValues(){
            /**
             * open new assessment
             */
            window.open("https://profiles.innermetrix.com/VO/a38ab06f/en");

            /**
             * redirect page
             */
            window.location.href = "/apply/assessment/?uuid="+this.uuid+"&stats=opened_link";

        },
        submit(){
            this.reset();
            // this.calculateAssessment();

            let userid = this.uuid;
            let self = this;

            console.log('fillers -> ', this.fillers);
            console.log('y-assessment answers -> ', this.answers);

            this.hideSubmit = false;
            this.aftersubmitloading = true;

            /**
                Political - high
                    average: 33 - 45
                    high: 46 - 53
                    very high: 54+
                
                Regulatory - high
                    average: 28 - 35
                    high: 36 - 42
                    very high: 43+

                theoretical - average 
                    average: 31 - 42
                    high: 43 - 54
                    very high: 55+
            */

            // check if user passed or not

            console.log('Political -> ', this.answers['political']);
            console.log('Regulatory -> ', this.answers['regulatory']);
            console.log('Theoretical -> ', this.answers['theoretical']);

            // let assesment_status = ((this.answers['political'] / 80 * 100) >= 46 && (this.answers['regulatory'] / 80 * 100) >= 36 && (this.answers['theoretical'] / 80 * 100) >= 31 ? 'InitialInterview' : "YAssessmentDone");
            let assesment_status = 'InitialInterview';
            
            const headers = {
                'content-type': 'application/json',
                'Access-Control-Allow-Origin': '*',
                'token': userid
            }

            // let params = {
            //     "yassessment":{
            //         "altruist": (this.answers['altruist'] / 80 * 100),
            //         "individualistic": (this.answers['individualistic'] / 80 * 100),
            //         "economic": (this.answers['economic'] / 80 * 100),
            //         "aesthetic": (this.answers['aesthetic'] / 80 * 100),
            //         "theoretical": (this.answers['theoretical'] / 80 * 100),
            //         "regulatory": (this.answers['regulatory'] / 80 * 100),
            //         "political": (this.answers['political'] / 80 * 100)
            //     },
            //     "status": assesment_status
            // };

            let params = {
                "yassessment":{
                    "altruist": 40,
                    "individualistic": 40,
                    "economic": 40,
                    "aesthetic": 40,
                    "theoretical": 40,
                    "regulatory": 40,
                    "political": 40,
                },
                "status": assesment_status
            };

            // console.log('submit this -> ', params);

            this.$axios.post('https://be2.applytocyberbacker.com/api/assessment/step/3', params, {headers: headers})
            .then(function (response) {
                console.log('yassessment -> ', response);
                console.log('duser -> ', userid);
                let param = {
                    "id": userid
                }
                
                self.$axios.post('https://be2.applytocyberbacker.com/api/assessment/savetogsheet', param, { headers: headers })
                .then(function (response) {
                    console.log('saving to g sheet -> ', userid);
                    console.log('saving to g sheet -> ', response);
                    localStorage.removeItem('swesomelogin');
                    self.$router.push('/apply/confirmation?uuid='+userid)

                }).catch(function (error) {
                    console.log('show error ->', error);
                });

                self.$router.push('/apply/confirmation?uuid='+userid)
                
            })
            .catch(function (error) {
                console.log('show error ->', error);
            });


        },
        reset(){
            this.answers = {
                individualistic: 0,
                theoretical: 0,
                aesthetic: 0,
                economic: 0,
                regulatory: 0,
                political: 0,
                altruist: 0,
            };

            this.fillers = [];
        },
        calculateAssessment(){
            let testq = this.bquest;
            console.log('this -> ', testq);
            for (let ndex in testq) {
                if(testq[ndex].atype == "assessment"){
                    const assessans = testq[ndex].answers;
                    for (let asset in assessans) {
                        let dtype = assessans[asset].atype;
                        dtype = dtype.toLowerCase().trim();
                        let dnum = 0;
                        if(asset == 0){ dnum = 7; }
                        else if(asset == 1){ dnum = 6; }
                        else if(asset == 2){ dnum = 5; }
                        else if(asset == 3){ dnum = 4; }
                        else if(asset == 4){ dnum = 3; }
                        else if(asset == 5){ dnum = 2; }
                        else if(asset == 6){ dnum = 1; }
                        this.answers[dtype] = parseInt(this.answers[dtype]) + (parseInt(dnum) + 1);
                        // console.log(dtype+" -> "+answers[dtype]);
                    }
                } else {
                    this.fillers.push(testq[ndex]);
                }
            }
            
        },
        onEnd(evt){ // changes on the arrangement of the yassessment answers
            // console.log(this.myArray);
            this.oldIndex = evt.oldIndex;
            this.newIndex = evt.newIndex;
            this.hasElementReArranged = true;
        },
        getQuestions(){ // get list of questions for the y-assessment
            let self = this;

            const headers = {
                'Access-Control-Allow-Origin': '*',
            }

            let params = {
                segment: 'random'
            }

            this.$axios.get('https://be2.applytocyberbacker.com/api/assessment/questions', {params}, { headers: headers })
            .then(function (response) {
                self.bquest = response.data.data.questions;
                console.log('questions -> ', response.data.data.questions);
                let questionsBase = response.data.data.questions;
                console.log('max counr -> ', questionsBase.length);

                self.maxIndex = questionsBase.length - 1;
                self.showQuestionsLoading = false;
            })
            .catch(function (error) {
                console.log('show error ->', error);
            });

        },
        proceedToNextQuestion(questionItem){ // proceed to next question request
            console.log('show -> ', questionItem);
            console.log('max -> ', this.maxIndex);
            

            if(this.hasElementReArranged){
                this.nextTextBase = 'Are you sure with your current arrangement? By proceeding, you cannot go back to the previous question.';
            } else {
                this.nextTextBase = 'You have not move any tile, Are you sure you want to proceed?';
            }
        },
        confirmProceedtoNextQuestion(questionItem){ // proceed to question confirmation
            this.panel = questionItem + 1;
            this.currentQuestions = questionItem + 1;

            console.log('max questions -> ', this.maxIndex);
            console.log('current questions -> ', questionItem);

            if(questionItem == this.maxIndex){
                console.log('show submit');
                this.showLoading = false;
                this.hideSubmit = true;
            }

            this.hasElementReArranged = false;
            this.nextTextBase = '';
        },
        checkValidUser(userID){
            console.log('browser token ', userID);

            if(userID == 'sb001'){
                this.validUser = false;
            } else if(typeof(userID) === 'undefined'){
                this.validUser = false;
            } else {
                this.validUser = true;
            }
            this.userID = userID;
            
        },
        setupUserID(){
            this.uuid = this.$route.query.uuid;

            if (typeof this.$route.query.stats === "undefined") {
                this.stats = false;
            } else {
                this.stats = true;
            }
        },
    },
    mounted(){
        // this.checkValidUser(this.$route.query.uuid);
        // this.getQuestions();
        this.setupUserID();
    }
}
</script>

<style scoped>
    .d-start-appliation {
        font-family: 'Poppins';
        /* text-align: center; */
        max-width: 950px;
        margin: 50px auto;
    }
    .d-start-header {
        margin-top: 60px;
        margin-bottom: 60px;
        text-align: center;
    }
    .d-start-header h1 {
        text-shadow: 4px 4px 8px #ccc;
        font-family: 'Arimo', sans-serif;
        letter-spacing: 2px;
        font-size: 75px;
        line-height: 1em;
    }
    .dwelcomeheader {
        text-align: center;
        margin-bottom: 50px;
        font-size: 14px;
    }

    .newquestions .hide-check {
        display: none;
    }

    .dquestiontitle {
        font-size: 18px;
    }

    .dquestionsuntitle {
        font-size: 14px;
        font-style: italic;
    }

    .d-question-title {
        font-family: 'Special Elite';
    }

    .danswers {
        margin-top: 25px;
    }

    .danswers .dnote {
        background: #4f4f4f;
        padding: 5px 10px;
        line-height: 1em;
        border-radius: 5px;
        font-size: 14px;
        margin-bottom: 5px;
        color: #fff;
    }

    .danswers .sortable {
        background: #eaeaea;
        margin-bottom: 5px;
        padding: 7px 10px;
        line-height: 1em;
        font-size: 14px;
        border-radius: 5px;
        cursor: pointer;
    }

    button.dnextbtn {
        background: #4f4f4f;
        color: #fff;
        line-height: 1em;
        display: inline-block;
        font-size: 14px;
        padding: 10px 20px;
        border-radius: 50px;
    }

    .dnextquestion {
        text-align: right;
        margin-top: 15px;
    }

    .dproceedMessage {
        font-size: 14px;
        text-align: right;
        margin-top: 15px;
    }

    .dproceedMessage button {
        background: #4f4f4f;
        color: #fff;
        line-height: 1em;
        display: inline-block;
        font-size: 14px;
        padding: 10px 20px;
        border-radius: 50px;
    }

    .subs {
        text-align: right;
        margin-top: 15px;
    }

    .subs button {
        background: #4f4f4f;
        color: #fff;
        line-height: 1em;
        display: inline-block;
        font-size: 14px;
        padding: 10px 20px;
        border-radius: 50px;
    }

    .show-loading-after-submit {
        text-align: center;
    }
    .show-loading-after-submit img{
        width: 450px;
        margin-top: -90px;
    }

    .submit-values {
        text-align: center;
    }

    .submit-values button {
        background: #1976d2;
        color: #fff;
        line-height: 1em;
        font-weight: 100;
        font-size: 25px;
        padding: 20px 60px;
        border-radius: 100px;
    }

</style>

<style> 
    body {
        background: #fff !important;
    }
</style>