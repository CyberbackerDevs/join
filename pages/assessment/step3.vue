<template>
    <div class="step_one">
        <v-app fluid>
            <div class="baseinfo">
                <div class="showcanttakeexam" v-if="!validUser">
                    <div class="scinner">
                        <h3>Page Restricted</h3>
                        <div class="dsmessage">You are not allowed to access the y-assessment. Please try again later</div>
                    </div>
                </div>
                <div class="allowshowform" v-if="validUser">
                    <div class="dwelcomeheader" >
                        <h3>Welcome to the Y-assessment</h3>
                        <div class="subwh">The Y assessment help understand the reasons that drive an individual to utilize<br />their talents in the unique way they do. This will determine your motivators and drivers.</div>
                        <div class="subwh">There will be different list of items for each questions. Please arrange the options<br />that represent your thoughts by clicking and dragging your “<strong>most preferential</strong>” answer to “<strong>least preferential</strong>”.</div>
                        <div class="subwh">It will take you about 15 minutes to complete the assessment.<br />Please find a quiet place where you will not be interrupted before starting.</div>
                    </div>
                    <div class="new_list">
                        <div class="dloadingpart" v-if="showLoading">
                            <div class="dlp-inner">
                                <v-progress-circular
                                    :size="70"
                                    indeterminate
                                    color="primary"
                                ></v-progress-circular>
                            </div>
                        </div>
                        <div class="newquestions" v-if="!showLoading">
                            <v-expansion-panels v-model="panel" >
                                <v-expansion-panel v-for="(item, index) in bquest" :key="item.avalue" :class="(currentQuestions <= index ? '' : 'question-done')" :disabled="(currentQuestions == index ? false : true)" multiple>
                                    <v-expansion-panel-header >
                                        Question {{index + 1}}
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
                                            <!-- <button class="dprevbtn" v-on:click="proceedToPrevQuestion(index)" :class="(index == 0 ? 'hide-button' : '')"><span><font-awesome-icon icon="arrow-left" /></span> Previous Question</button> -->
                                            <button class="dnextbtn" v-on:click="proceedToNextQuestion(index)" :class="(currentQuestions == maxIndex ? 'hide-button' : '')">Next Question <span><font-awesome-icon icon="arrow-right" /></span></button>
                                        </div>
                                    </v-expansion-panel-content>
                                </v-expansion-panel>
                            </v-expansion-panels>
                        </div>
                    </div>
                    <div class="dsubmitforms" v-if="!showLoading">
                        <div class="subs" v-if="hideSubmit">
                            <button type="submit" v-on:click="submit">Submit Y-assessment</button>
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

    import { library } from '@fortawesome/fontawesome-svg-core'
    import { faCheckCircle} from '@fortawesome/free-regular-svg-icons'
    import { faArrowRight, faArrowLeft } from '@fortawesome/free-solid-svg-icons'
    import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'

    library.add(faCheckCircle, faArrowRight, faArrowLeft)

    export default {
        head: {
            title: 'Y Assessment',
        },
        name: 'step_three',
        layout: 'StageOne',
        components: {
            draggable,
            FontAwesomeIcon
        },
        data(){
            return {
                hideSubmit: false,
                panel: 0,
                currentQuestions: 0,
                validUser: false,
                maxIndex: 0,
                myArray: [
                    {name: 'Angular', id: 0},
                    {name: 'React', id: 1},
                    {name: 'Vue', id: 2},
                    {name: 'HTML', id: 3},
                    {name: 'PHP', id: 4},
                ],
                bquest: [],
                oldIndex: '',
                newIndex: '',
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
                userID: '',
                showLoading: true,
                uuid: '',
                hasElementReArranged: false,
                nextTextBase: ''
            }
        },
        methods: {
            submit(){
                this.reset();
                this.calculateAssessment();

                let userid = this.uuid;
                let self = this;

                console.log('fillers -> ', this.fillers);
                console.log('y-assessment answers -> ', this.answers);

                this.showLoading = true;

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

                let params = {
                    "yassessment":{
                        "altruist": (this.answers['altruist'] / 80 * 100),
                        "individualistic": (this.answers['individualistic'] / 80 * 100),
                        "economic": (this.answers['economic'] / 80 * 100),
                        "aesthetic": (this.answers['aesthetic'] / 80 * 100),
                        "theoretical": (this.answers['theoretical'] / 80 * 100),
                        "regulatory": (this.answers['regulatory'] / 80 * 100),
                        "political": (this.answers['political'] / 80 * 100)
                    },
                    "status": assesment_status
                };

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
                        self.$router.push('/assessment/thankyou?uuid='+userid)

                    }).catch(function (error) {
                        console.log('show error ->', error);
                    });
                    
                })
                .catch(function (error) {
                    console.log('show error ->', error);
                });


            },
            calculateAssessment(){
                let testq = this.bquest;

                for (let ndex in testq) {
                    if(testq[ndex].atype == "assessment"){
                        const assessans = testq[ndex].answers;
                        for (let asset in assessans) {
                            const dtype = assessans[asset].atype;
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
            onEnd(evt){
                // console.log(this.myArray);
                this.oldIndex = evt.oldIndex;
                this.newIndex = evt.newIndex;
                this.hasElementReArranged = true;
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
            getQuestions(){
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
                    self.showLoading = false;
                })
                .catch(function (error) {
                    console.log('show error ->', error);
                });

            },
            setupUserID(){
                this.uuid = this.$route.query.uuid;
            },
            proceedToPrevQuestion(questionItem){
                this.panel = questionItem - 1;
                this.currentQuestions = questionItem - 1;
                this.hideSubmit = false;
            },
            proceedToNextQuestion(questionItem){
                console.log('show -> ', questionItem);
                console.log('max -> ', this.maxIndex);
                

                if(this.hasElementReArranged){
                    this.nextTextBase = 'Are you sure with your current arrangement? By proceeding, you cannot go back to the previous question.';
                } else {
                    this.nextTextBase = 'You have not move any tile, Are you sure you want to proceed?';
                }

                // this.panel = questionItem + 1;
                // this.currentQuestions = questionItem + 1;

                // if(questionItem + 1 == this.maxIndex){
                //     this.hideSubmit = true;
                // }
            },
            confirmProceedtoNextQuestion(questionItem){
                this.panel = questionItem + 1;
                this.currentQuestions = questionItem + 1;

                if(questionItem + 1 == this.maxIndex){
                    this.hideSubmit = true;
                }

                this.hasElementReArranged = false;
                this.nextTextBase = '';
            }
        },
        mounted() {
            this.checkValidUser(this.$route.query.uuid);
            this.getQuestions();
            this.setupUserID();
        }
    }
</script>

<style scoped>
    .dpositionitem {
        width: 25%;
        display: inline-block;
    }
    .danswers {
        /* background: #eee;
        padding: 10px;
        border-radius: 5px;
        border: 1px solid #cecece; */
    }
    .dwelcomeheader {
        text-align: center;
        margin-bottom: 40px;
    }
    .dwelcomeheader h3 {
        font-size: 31px;
        font-weight: 400;
    }
    .dwelcomeheader .subwh {
        font-size: 13px;
        width: 80%;
        margin: 0 auto 10px;
    }
    .dquestiontitle {
        font-size: 24px;
        line-height: 1.3em;
        
    }
    .dquestionsuntitle {
        font-size: 14px;
        line-height: 1.3em;
        margin-bottom: 16px;
    }
    .new_list .dlsit {
        margin-bottom: 30px;
    }
    .sortable {
        border: 1px solid #e3e6e8;
        border-radius: 5px;
        padding: 5px 10px;
        margin-bottom: 5px;
        cursor: pointer;
    }
    .sortable:hover {
        background: #4eacdf;
        color: #fff;
    }
    .danswers .dnote {
        text-align: center;
        margin: 10px 0;
        font-size: 14px;
        font-style: italic;
    }

    .theme--light.v-application {
        background: none;
    }
    
    .step_one {
        background: #edf8fc;
        padding: 50px 60px;
        border-radius: 10px;
        opacity: .9;
        font-family: 'Poppins' !important;
    }

    .dforminfo .row.mb-6 {
        margin-bottom: 0 !important;
        margin-top: 0 !important;
    }

    .form-item label {
        display: block;
        font-size: 13px;
        color: #7a7b7d;
        line-height: 1em;
        margin-bottom: 10px;
    }

    .form-item input[type="text"], .form-item textarea {
        display: block;
        width: 100%;
        background: #fff;
        color: #7a7b7d;
        padding: 15px 18px;
        line-height: 1em;
        border-radius: 7px;
        border: 1px solid #d8d8d8;
    }
    .step_one h2 {
        font-weight: 400;
        line-height: 1em;
        margin-bottom: 5px;
    }

    .step_one .subheads {
        font-size: 14px;
        line-height: 1em;
        margin-bottom: 25px;
    }

    .dsubmitforms button {
        margin-top: 25px;
        background: #0896d8;
        border-bottom: 4px solid #0075aa;
        padding: 15px 30px;
        color: #fff;
        line-height: 1em;
        border-radius: 10px;
    }
    .v-input--selection-controls {
        margin: 0 !important;
        padding: 0 !important;
    }
    .dformitem {
        margin-bottom: 20px;
    }
    .scinner {
        text-align: center;
    }
    .scinner h3 {
        margin-top: 30px;
        font-size: 47px;
        font-weight: 400;
    }
    .dloadingpart {
        text-align: center;
    }

    .hide-check {
        display: none;
    }
    .dnextquestion .hide-button {
        display: none;
    }
    .dnextquestion {
        text-align: right;
    }
    .dnextquestion button.dprevbtn {
        background: #4d64a7;
        background: linear-gradient(137deg, #485b96 0%, #4d64a7 100%);
        color: #fff;
        font-size: 12px;
        /* font-weight: lighter; */
        line-height: 1em;
        padding: 10px 20px;
        border-radius: 20px;
        margin-right: 10px;
    }
    .dnextquestion button.dnextbtn {
        background: #fe608b;
        background: linear-gradient(137deg, #fe608b 0%, #ff99a2 100%);
        color: #fff;
        font-size: 12px;
        /* font-weight: lighter; */
        line-height: 1em;
        padding: 10px 20px;
        border-radius: 20px;
        /* margin-right: 15px; */
    }
    .question-done {
        background: #adffab !important;
    }
    .question-done button {
        color: #106419;
    }

    .dproceedMessage > * {
        display: inline-block;
        vertical-align: middle;
    }
    .dproceedMessage {
        text-align: right;
    }

    .dproceedMessage span{
        font-size: 12px;
    }

    .dproceedMessage button {
        background: #fe608b;
        background: linear-gradient(137deg, #fe608b 0%, #ff99a2 100%);
        color: #fff;
        font-size: 12px;
        line-height: 1em;
        padding: 10px 20px;
        border-radius: 20px;
        margin-left: 10px;
    }
    

</style>