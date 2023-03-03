<template>
    <div class="show-assessment-settings">
        <div class="sa-question-list">
            <div class="sa-add-question">
                <button v-on:click="showAllSegments" v-if="selectedSegment != ''">< Back</button>
                <button v-on:click="addSetDocu" v-if="!showAddSet  && selectedSegment == ''">Add New Question Set</button>
                <button v-on:click="addQuestionDocu" v-if="!showAddQuestions && selectedSegment != ''">Add Question Item</button>
                <div class="daddquestions" v-if="showAddQuestions">
                    <h2>Add Question</h2>
                    <div class="addquestionfields">
                        <div class="saq-item sa-question-type">
                            <label for="">Type</label>
                            <select v-model="addQuestion.atype">
                                <option value="filler">Filler</option>
                                <option value="assessment">Assessment</option>
                            </select>
                        </div>
                        <div class="saq-item sa-question-title">
                            <label for="">Title</label>
                            <textarea v-model="addQuestion.avalue"></textarea>
                        </div>
                        <div class="saq-item sa-question-translation">
                            <label for="">Translation</label>
                            <textarea v-model="addQuestion.aref"></textarea>
                        </div>
                        <div class="saq-save">
                            <button class="cancelbutton" v-on:click="closeQuestion">Cancel</button>
                            <button class="savebutton" v-on:click="saveQuestion">Save Question</button>    
                        </div> 
                    </div>
                </div>
                <div class="daddquestions" v-if="showAddSet">
                    <h2>Add Set</h2>
                    <div class="addquestionfields">
                        <div class="saq-item sa-question-title">
                            <label for="">Title</label>
                            <textarea v-model="addSegmentValues.setName"></textarea>
                        </div>
                        <div class="saq-item sa-question-translation">
                            <label for="">Description</label>
                            <textarea v-model="addSegmentValues.setDesc"></textarea>
                        </div>
                        <div class="saq-save">
                            <button class="cancelbutton" v-on:click="closeSetDocu">Cancel</button>
                            <button class="savebutton" v-on:click="saveSetDocu">Save Question Set</button>    
                        </div> 
                    </div>
                </div>
            </div>
            <div v-if="selectedSegment == ''" class="sa-segments-item-icons">
                <div class="sa-segment-item" v-for="(segment, sindex) in segmentList" :key="sindex">
                    <h4 v-on:click="assignSegmentID(segment.id)">{{segment.segment_title}}</h4>
                    <div class="sa-description" v-on:click="assignSegmentID(segment.id)">{{segment.segment_desc}}</div>
                    <div class="sa-is-active">{{ segment.isactive == "1" ? "Active": "Inactive" }}</div>
                    <div class="sa-is-switch">
                        <v-switch @change="activateSegment($event, segment.id)" :input-value="(segment.isactive == '1' ? true : false)" dense></v-switch>
                    </div>
                    <!-- <div class="sa-button">
                        <button v-on:click="assignSegmentID(segment.id)">View Question Set</button>
                    </div> -->
                </div>
            </div>
            <div  v-if="selectedSegment != ''" class="sa-questions-item-list" >
                <h2>List of questions - {{selectedSegment}}</h2>
                <!-- <div class="saq-sub-title"><strong>{{questions.length}}</strong> active questions</div> -->
                <div class="sa-questions-item sa-qestions-title">
                    <div class="dq-title">Question</div>
                    <div class="dq-type">Type</div>
                    <div class="dq-action">&nbsp;</div>
                </div>
                <div class="sa-questions-item" v-on:click="selectAQuestion(items.id, items)" v-for="(items, index) in questions" :key="index">
                    <div class="sa-item-name">{{items.avalue}}</div>
                    <div class="sa-item-type">{{items.atype}}</div>
                    
                    <div class="sa-item-action">></div>
                </div>
                <div class="sa-questions-item sa-qestions-title">
                    <div class="dq-title">Question</div>
                    <div class="dq-type">Type</div>
                    <div class="dq-action">&nbsp;</div>
                </div>
            </div>
        </div>
        <div class="sa-question-details">
            <div class="sa-question-info" v-if="showAnswers">
                <div class="sa-question-info-inner" v-if="!showAnswersLoading">
                    <h3>Question Details</h3>
                    <div class="dquestiondetails">
                        <div class="saq-item sa-question-type">
                            <label for="">Type</label>
                            <select v-model="selectedQuestion.atype">
                                <option value="filler">Filler</option>
                                <option value="assessment">Assessment</option>
                            </select>
                        </div>
                        <div class="saq-item sa-question-title">
                            <label for="">Title</label>
                            <textarea v-model="selectedQuestion.avalue"></textarea>
                        </div>
                        <div class="saq-item sa-question-translation">
                            <label for="">Translation</label>
                            <textarea v-model="selectedQuestion.aref"></textarea>
                        </div>
                        <div class="saq-delete">
                            <button v-on:click="dDeleteDetails">Delete this Question</button>    
                        </div> 
                        <div class="saq-save">
                            <button v-on:click="dSaveDetails">Save Details</button>    
                        </div> 
                    </div>
                </div> 
                <div class="sa-questions-loading" v-if="showAnswersLoading">
                    <v-progress-circular indeterminate color="primary" ></v-progress-circular>
                </div>
            </div>
            <div class="sa-add-question" v-if="showAddAnswers">
                <h2>Add Answer</h2>
                <div class="sa-add-question-form">
                    <div class="sa-ans sa-ans-type">
                        <label for="">Type</label>
                        <select v-model="addAnswers.axtype">
                            <option v-for="(sansitem, sansindex) in answersType" :key="sansindex" :value="sansitem">{{sansitem}}</option>
                        </select>
                        <div class="daddAnswersTypeError derrorpart" v-if="addAnswersTypeError != ''">{{addAnswersTypeError}}</div>
                    </div>
                    <div class="sa-ans sa-ans-value">
                        <label for="">Answer</label>
                        <textarea v-model="addAnswers.axvalue"></textarea>
                        <div class="daddAnswersValueError derrorpart" v-if="addAnswersValueError != ''">{{addAnswersValueError}}</div>
                    </div>
                    <div class="sa-add-ops">
                        <button class="cancelbutton" v-on:click="closeAnswers">Cancel</button>
                        <button class="savebutton" v-on:click="saveAnswers">Insert Answer</button>    
                    </div>
                </div>
            </div>
            <div class="sa-answers-conts" >
                <div class="sa-no-answers" v-if="!showAnswers">Please select a question</div>
                <div class="sa-answers-list" v-if="showAnswers">
                    <div class="sa-answers-loading" v-if="showAnswersLoading">
                        <v-progress-circular indeterminate color="primary" ></v-progress-circular>
                    </div>
                    <div class="sa-answers-items" v-if="!showAnswersLoading">
                       <div class="dheaderpart">
                           <h3>Answers Details</h3>
                           <div class="daddanswer">
                               <button v-on:click="addAnswer"  v-if="!showAddAnswers">Add Answer</button>
                           </div>
                           <div class="dsaveans">
                               <button v-on:click="InjectAnswers"  v-if="answers.length !== 0">Save Answers</button>
                           </div>
                       </div>
                       <div class="sa-ans-list">
                           <div class="sa-ans-item" v-for="(ansitem, ansindex) in answers" :key="ansindex">
                               <div class="sa-ans sa-ans-type">
                                   <label for="">Type</label>
                                    <select v-model="ansitem.atype">
                                        <option v-for="(sansitem, sansindex) in answersType" :key="sansindex" :value="sansitem">{{sansitem}}</option>
                                    </select>
                               </div>
                               <div class="sa-ans sa-ans-options">
                                    <button v-on:click="deleteAnswer(ansindex)">Delete Answer</button>   
                                </div> 
                               <div class="sa-ans sa-ans-value">
                                   <label for="">Answer</label>
                                   <textarea v-model="ansitem.avalue"></textarea>
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
    data(){
        return {
            showAddAnswers: false,
            showAddQuestions: false,
            showAddSet: false,
            showAnswersLoading: false,
            showAnswers: false,
            questions: [],
            selectedQuestion: [],
            answers: [],
            answersType: [],
            addQuestion: {
                atype: '',
                avalue: '',
                aref: '',
            },
            addAnswers: {
                axtype: '',
                axvalue: ''
            },
            listedAnswers: [],
            addAnswersTypeError: '',
            addAnswersValueError: '',
            selectedSegment: '',
            segmentList: [],
            addSegmentValues: {
                setName: '',
                setDesc: ''
            }
        }
    },
    methods: {
        resetErrorAnswers(){
            this.addAnswersTypeError = '';
            this.addAnswersValueError = '';
        },
        resetAddAnswers(){
            this.addAnswers.axtype = '';
            this.addAnswers.axvalue = '';
        },
        closeAnswers(){
            this.showAddAnswers = false;
        },
        addAnswer(){
            this.showAddAnswers = true;
        },
        dDeleteDetails(){
            console.log('delete the question');

            let self = this;

            let header = {
                "Access-Control-Allow-Origin": "*",
                "Access-Control-Allow-Methods": "GET, POST, PATCH, PUT, DELETE, OPTIONS",
                "Access-Control-Allow-Headers": "Origin, Content-Type, X-Auth-Token"
            }

            let params = {
                "question": this.selectedQuestion.id,
            }

            this.$axios.post('https://be2.applytocyberbacker.com/api/options/assessment/question/delete', params, { headers: header })
            .then(function (response) {
                // console.log('info -> ', response.data.data);

                self.showAddAnswers = false;
                self.showAddQuestions = false;
                self.showAnswersLoading = false;
                self.showAnswers = false;
                self.selectedQuestion = [];
                self.answers = [];

                self.getQuestions();
                
            })
            .catch(function (error) {
                console.log(error);
            });

        },
        dSaveDetails(){
            console.log('save details ->', this.selectedQuestion);

            let self = this;

            let header = {
                "Access-Control-Allow-Origin": "*",
                "Access-Control-Allow-Methods": "GET, POST, PATCH, PUT, DELETE, OPTIONS",
                "Access-Control-Allow-Headers": "Origin, Content-Type, X-Auth-Token"
            }

            let params = {
                "id": this.selectedQuestion.id,
                "atype": this.selectedQuestion.atype,
                "aindex": "question",
                "aref": this.selectedQuestion.aref,
                "avalue": this.selectedQuestion.avalue
            }

            console.log('params ->', params);

            this.$axios.post('https://be2.applytocyberbacker.com/api/options/assessment/question/update', params, { headers: header })
            .then(function (response) {
                // console.log('info -> ', response.data.data);

                self.selectAQuestion(self.selectedQuestion.id, self.selectedQuestion);
                
            })
            .catch(function (error) {
                console.log(error);
            });


        },
        deleteAnswer(selectedIndex){
            console.log('selected item ->', selectedIndex);

            this.answers.splice(selectedIndex, 1);
        },
        InjectAnswers(){
            console.log('save answers ->', this.answers);
            console.log('save answers ->', this.selectedQuestion.id);

            let self = this;

            let header = {
                "Access-Control-Allow-Origin": "*",
                "Access-Control-Allow-Methods": "GET, POST, PATCH, PUT, DELETE, OPTIONS",
                "Access-Control-Allow-Headers": "Origin, Content-Type, X-Auth-Token"
            }

            let params = {
                "answers": this.answers,
                "reference_id": this.selectedQuestion.id
            }

            this.$axios.post('https://be2.applytocyberbacker.com/api/options/assessment/answer/update', params, { headers: header })
            .then(function (response) {
                // console.log('info -> ', response.data.data);

                self.getQuestions();
                self.selectAQuestion(self.selectedQuestion.id, self.selectedQuestion);

                
            })
            .catch(function (error) {
                console.log(error);
            });

        },
        saveAnswers(){
            console.log('save answer ->', this.addAnswers);
            this.resetErrorAnswers();

            if(this.addAnswers.axtype == ""){
                this.addAnswersTypeError = 'Please select an answer type';
                return;
            }

            if(this.addAnswers.axvalue == ""){
                this.addAnswersValueError = 'Answer value must not be empty';
                return;
            }

            let answerinfo = {
                atype: this.addAnswers.axtype,
                avalue: this.addAnswers.axvalue,
            };
            this.answers.push(answerinfo);

            this.resetAddAnswers();
        },
        saveQuestion(){
            console.log('save new question -> ', this.addQuestion);

            let self = this;

            let header = {
                "Access-Control-Allow-Origin": "*",
                "Access-Control-Allow-Methods": "GET, POST, PATCH, PUT, DELETE, OPTIONS",
                "Access-Control-Allow-Headers": "Origin, Content-Type, X-Auth-Token"
            }

            let params = {
                "atype": this.addQuestion.atype,
                "aindex": "question",
                "aref": this.addQuestion.aref,
                "avalue": this.addQuestion.avalue,
                "segment": this.selectedSegment
            }

            this.$axios.post('https://be2.applytocyberbacker.com/api/options/assessment/question/add', params, { headers: header })
            .then(function (response) {
                console.log('info -> ', response.data.data);

                self.getQuestions();
                self.showAddQuestions = false;
                self.addQuestion.atype = '';
                self.addQuestion.avalue = '';
                self.addQuestion.aref = '';
                
                // self.answers = response.data.data.answers;
                // self.showAnswersLoading = false;
                
            })
            .catch(function (error) {
                console.log(error);
            });
        },
        activateSegment(event, segmentID){
            console.log('event segmnet ->', event);
            console.log('segment id ->', segmentID);

            let self = this;

            let header = {
                "Access-Control-Allow-Origin": "*",
                "Access-Control-Allow-Methods": "GET, POST, PATCH, PUT, DELETE, OPTIONS",
                "Access-Control-Allow-Headers": "Origin, Content-Type, X-Auth-Token"
            }

            let triggeraction = (event ? 'activate' : 'deactivate');
            

            let params = {
                "segment": segmentID,
                "action": triggeraction
            }

            this.$axios.post('https://be2.applytocyberbacker.com/api/options/assessment/segment/activate', params, { headers: header })
            .then(function (response) {
                console.log('info -> ', response);

                self.getSegments();
                self.showAddSet = false;
                self.addSegmentValues.setName = '';
                self.addSegmentValues.setDesc = '';
            })
            .catch(function (error) {
                console.log(error);
            });
            
        },
        closeQuestion(){
            this.showAddQuestions = false;
        },
        saveSetDocu(){
            console.log('this is a test -> ', this.addSegmentValues);

            let self = this;

            let header = {
                "Access-Control-Allow-Origin": "*",
                "Access-Control-Allow-Methods": "GET, POST, PATCH, PUT, DELETE, OPTIONS",
                "Access-Control-Allow-Headers": "Origin, Content-Type, X-Auth-Token"
            }

            let params = {
                "title": this.addSegmentValues.setName,
                "desc": this.addSegmentValues.setDesc
            }

            this.$axios.post('https://be2.applytocyberbacker.com/api/options/assessment/segment/add', params, { headers: header })
            .then(function (response) {
                console.log('info -> ', response.data.data);

                self.getSegments();
                self.showAddSet = false;
                self.addSegmentValues.setName = '';
                self.addSegmentValues.setDesc = '';
            })
            .catch(function (error) {
                console.log(error);
            });
        },
        addQuestionDocu(){
            this.showAddQuestions = true;
            this.showAddSet = false;
        },
        addSetDocu(){
            this.showAddSet = true;
            this.showAddQuestions = false;
        },
        closeSetDocu(){
            this.showAddSet = false;
        },
        setAnswerTypes(questionType){
            if(questionType == 'assessment'){
                this.answersType = [
                    "altruist",
                    "aesthetic",
                    "theoretical",
                    "regulatory",
                    "individualistic",
                    "economic",
                    "political",
                ]
            } else {
                this.answersType = ["filler"]
            }
        },
        selectAQuestion(qid, item){
            console.log('selected question id -> ', qid);

            this.selectedType = item.atype;
            this.setAnswerTypes(item.atype);
            this.selectedQuestion = item;
            this.showAnswers = true;
            this.showAnswersLoading = true;

            let self = this;

            let header = {
                "Access-Control-Allow-Origin": "*",
                "Access-Control-Allow-Methods": "GET, POST, PATCH, PUT, DELETE, OPTIONS",
                "Access-Control-Allow-Headers": "Origin, Content-Type, X-Auth-Token"
            }

            let params = {
                "question": qid
            }

            this.$axios.post('https://be2.applytocyberbacker.com/api/assessment/answers', params, { headers: header })
            .then(function (response) {
                console.log('info -> ', response.data.data);
                
                self.answers = response.data.data.answers;
                self.showAnswersLoading = false;
                
            })
            .catch(function (error) {
                console.log(error);
            });

        },
        getQuestions(){
            console.log('pull questions');

            let self = this;

            let header = {
                "Access-Control-Allow-Origin": "*",
                "Access-Control-Allow-Methods": "GET, POST, PATCH, PUT, DELETE, OPTIONS",
                "Access-Control-Allow-Headers": "Origin, Content-Type, X-Auth-Token"
            }

            let params = {
                segment: this.selectedSegment
            }

            this.$axios.get('https://be2.applytocyberbacker.com/api/assessment/questions', {params}, { headers: header })
            .then(function (response) {
                console.log('info -> ', response.data.data);
                
                self.questions = response.data.data.questions;
                
            })
            .catch(function (error) {
                console.log(error);
            });
        },
        getSegments(){
            console.log('get segmets');

            let self = this;

            let header = {
                "Access-Control-Allow-Origin": "*",
                "Access-Control-Allow-Methods": "GET, POST, PATCH, PUT, DELETE, OPTIONS",
                "Access-Control-Allow-Headers": "Origin, Content-Type, X-Auth-Token"
            }

            this.$axios.get('https://be2.applytocyberbacker.com/api/options/assessment/segment/get', {}, { headers: header })
            .then(function (response) {
                console.log('segment info -> ', response.data.data);
                
                self.segmentList = response.data.data.segments;
                
            })
            .catch(function (error) {
                console.log(error);
            });
        },
        assignSegmentID(id){
            console.log(id);
            this.selectedSegment = id;
            this.getQuestions();
        },
        showAllSegments(){
            this.selectedSegment = '';
            this.questions = '';
            this.getSegments();
        }
    },
    mounted(){
        // this.getQuestions();
        this.getSegments();
    }
}
</script>

<style scoped>
    .show-assessment-settings .sa-question-list {
        display: inline-block;
        width: 48%;
        margin-right: 2%;
        vertical-align: top;
    }
    .show-assessment-settings .sa-question-details {
        display: inline-block;
        width: 49%;
        vertical-align: top;
    }
    .show-assessment-settings .sa-question-details .sa-question-info .sa-questions-loading {
        text-align: center;
        padding: 60px 0;
    }
    .show-assessment-settings .sa-question-list .sa-questions-item-list {
        background: #fff;
        border-radius: 10px;
        overflow: hidden;
        /* padding: 20px; */
        box-shadow: 0 0 12px #33333360;
        margin-bottom: 25px;
    }
    .sa-questions-item-list .sa-questions-item  {
        padding: 10px 15px;
        border-bottom: 1px solid #ccc;
        font-size: 12px;
        cursor: pointer;
    }
    .sa-questions-item-list .sa-questions-item.sa-qestions-title {
        background: #ecddf1;
        cursor: auto;
    }
    .sa-questions-item-list .sa-questions-item.sa-qestions-title .dq-title {
        display: inline-block;
        vertical-align: middle;
        width: 70%;
        font-weight: bold;
        text-transform: uppercase;
    }
    .sa-questions-item-list .sa-questions-item.sa-qestions-title .dq-type {
        display: inline-block;
        vertical-align: middle;
        width: 20%;
        font-weight: bold;
        text-transform: uppercase;
    }
    .sa-questions-item-list .sa-questions-item.sa-qestions-title .dq-action {
        display: inline-block;
        vertical-align: middle;
        width: 8%;
    }
    .sa-questions-item-list .sa-questions-item:last-child {
        border-bottom: 0 none;
    }
    .sa-questions-item-list .sa-questions-item .sa-item-type {
        display: inline-block;
        width: 100%;
        vertical-align: middle;
        line-height: 1em;
    }
    .sa-questions-item-list .sa-questions-item .sa-item-name {
        display: inline-block;
        width: 70%;
        vertical-align: middle;
        line-height: 1.3em;
    }
    .sa-questions-item-list .sa-questions-item .sa-item-type {
        display: inline-block;
        width: 20%;
        vertical-align: middle;
        text-transform: capitalize;
    }
    .sa-questions-item-list .sa-questions-item .sa-item-action {
        display: inline-block;
        width: 8%;
        vertical-align: middle;
        text-align: right;
    }
    .sa-no-answers {
        text-align: center;
        padding: 130px;
    }
    .sa-answers-loading {
        text-align: center;
        padding: 130px;
    }
    .sa-answers-conts {
        border-radius: 10px;
        background: #fff;
        overflow: hidden;
        box-shadow: 0 0 12px #33333360;
    }

    .show-assessment-settings .sa-question-info .sa-question-info-inner {
        width: 100%;
        background: #fff;
        margin-bottom: 15px;
        border-radius: 10px;
        /* padding: 15px 20px; */
        overflow: hidden;
        box-shadow: 0 0 12px #33333360;
    }
    .show-assessment-settings .sa-question-info .sa-question-info-inner .dquestiondetails {
        padding: 15px 20px;
    }
    .show-assessment-settings .sa-question-info .sa-question-info-inner .saq-item {
        margin-bottom: 10px;
    }
    .show-assessment-settings .sa-question-info .sa-question-info-inner .saq-item label {
        width: 100%;
        display: block;
        font-size: 12px;
        font-weight: bold;
        margin-bottom: 5px;
    }
    .show-assessment-settings .sa-question-info .sa-question-info-inner .saq-item select {
        display: block;
        border: 1px solid #ecddf1;
        width: 100%;
        background: #ecddf1;
        line-height: 1em;
        padding: 10px 20px;
        border-radius: 5px;
        color: #4c5f99;
        font-size: 12px;
        font-weight: bold;
        appearance: auto;
    }
    .show-assessment-settings .sa-question-info .sa-question-info-inner .saq-item textarea {
        display: block;
        border: 1px solid #ecddf1;
        width: 100%;
        height: 80px;
        background: #ecddf1;
        line-height: 1.3em;
        padding: 10px 20px;
        border-radius: 5px;
        color: #4c5f99;
        font-size: 12px;
        font-weight: bold;
    }
    .show-assessment-settings .sa-question-info .sa-question-info-inner .saq-delete {
        display: inline-block;
        width: 50%;
        vertical-align: middle;
    }
    .show-assessment-settings .sa-question-info .sa-question-info-inner .saq-delete button {
        background: #ac4444;
        background: linear-gradient(137deg, #ac4444 0%, #ff7676 100%);
        color: #fff;
        line-height: 1em;
        font-size: 11px;
        padding: 10px 20px;
        border-radius: 30px;
    }
    .show-assessment-settings .sa-question-info .sa-question-info-inner .saq-save {
        display: inline-block;
        width: 49%;
        vertical-align: middle;
        text-align: right;
    }
    .show-assessment-settings .sa-question-info .sa-question-info-inner .saq-save button {
        background: #4d64a7;
        background: linear-gradient(137deg, #485b96 0%, #4d64a7 100%);
        color: #fff;
        line-height: 1em;
        font-size: 11px;
        padding: 10px 20px;
        border-radius: 30px;
    }
    .sa-answers-items .sa-ans-item .sa-ans.sa-ans-type label {
        display: inline-block;
        width: 10%;
        vertical-align: middle;
        font-weight: bold;
        font-size: 12px;
        line-height: 1em;
    }
    .sa-answers-items .sa-ans-item .sa-ans.sa-ans-type select {
        display: inline-block;
        width: 89%;
        vertical-align: middle;
        border: 1px solid #ecddf1;
        background: #ecddf1;
        line-height: 1em;
        padding: 10px 20px;
        border-radius: 5px;
        color: #4c5f99;
        font-size: 12px;
        font-weight: bold;
        appearance: auto;
    }
    .sa-answers-items .sa-ans-item .sa-ans.sa-ans-type {
        margin-bottom: 10px;
        display: inline-block;
        width: 74%;
        vertical-align: middle;
    }
    .sa-answers-items .sa-ans-item .sa-ans.sa-ans-options {
        margin-bottom: 10px;
        width: 24%;
        display: inline-block;
        vertical-align: middle;
        text-align: right;
    }
    .sa-answers-items .sa-ans-item .sa-ans.sa-ans-options button {
        background: #ac4444;
        background: linear-gradient(137deg, #ac4444 0%, #ff7676 100%);
        color: #fff;
        line-height: 1em;
        font-size: 11px;
        padding: 10px 20px;
        border-radius: 30px;
    }
    .sa-answers-items .sa-ans-item .sa-ans.sa-ans-value label {
        display: block;
    }
    .sa-answers-items .sa-ans-item .sa-ans.sa-ans-value textarea {
        display: block;
        width: 100%;
    }
    .sa-answers-items .sa-ans-item .sa-ans.sa-ans-value label {
        display: inline-block;
        width: 10%;
        vertical-align: top;
        font-weight: bold;
        font-size: 12px;
        line-height: 1em;
        margin-bottom: 5px;
    }
    .sa-answers-items .sa-ans-item .sa-ans.sa-ans-value textarea {
        display: inline-block;
        width: 89%;
        vertical-align: middle;
        border: 1px solid #ecddf1;
        background: #ecddf1;
        line-height: 1em;
        padding: 10px 20px;
        border-radius: 5px;
        color: #4c5f99;
        font-size: 12px;
        font-weight: bold;
    }
    .sa-answers-items .sa-ans-item {
        margin-bottom: 15px;
        padding-bottom: 15px;
        border-bottom: 1px solid #cccccc;
    }
    .sa-answers-items .sa-ans-item:last-child {
        border-bottom: 0 none;
    }
    .sa-answers-items .sa-ans-list {
        padding: 25px;
    }
    .sa-answers-items h3,
    .sa-add-question h2,
    .show-assessment-settings .sa-question-info .sa-question-info-inner h3,
    .show-assessment-settings .sa-question-list .sa-questions-item-list h2,
    .sa-add-question .daddquestions h2 {
        background: url('~/assets/images/placeheader.jpg') no-repeat center right;
        color: #fff;
        padding: 10px 25px 10px;
        font-size: 14px;
        font-weight: normal;
    }
    .sa-add-question {
        margin-bottom: 15px;
    }
    .sa-question-details .sa-add-question {
        margin-bottom: 15px;
        box-shadow: 0 0 12px #33333360;
        background: #fff;
        border-radius: 10px;
        overflow: hidden;
    }
    .sa-add-question button {
        background: #4d64a7;
        background: linear-gradient(137deg, #485b96 0%, #4d64a7 100%);
        color: #fff;
        line-height: 1em;
        font-size: 11px;
        padding: 10px 20px;
        border-radius: 30px;
    }
    .daddquestions button.savebutton {
        background: #fe608b;
        background: linear-gradient(137deg, #fe608b 0%, #ff99a2 100%);
    }
    .sa-add-question .daddquestions {
        box-shadow: 0 0 12px #33333360;
        border-radius: 10px;
        background: #fff;
        overflow: hidden;
        margin-top: 10px;
    }
    .sa-add-question .daddquestions .addquestionfields {
        padding: 15px 20px;
    }
    .sa-add-question .daddquestions .addquestionfields .saq-item {
        width: 100%;
        display: block;
        font-size: 12px;
        font-weight: bold;
        margin-bottom: 15px;
    }
    .sa-add-question .daddquestions .addquestionfields select {
        display: block;
        border: 1px solid #ecddf1;
        width: 100%;
        background: #ecddf1;
        line-height: 1em;
        padding: 10px 20px;
        border-radius: 5px;
        color: #4c5f99;
        font-size: 12px;
        font-weight: bold;
        appearance: auto;
    }
    .sa-add-question .daddquestions .addquestionfields textarea {
        display: block;
        border: 1px solid #ecddf1;
        width: 100%;
        height: 80px;
        background: #ecddf1;
        line-height: 1.3em;
        padding: 10px 20px;
        border-radius: 5px;
        color: #4c5f99;
        font-size: 12px;
        font-weight: bold;
    }
    .sa-add-question .daddquestions .addquestionfields .saq-save {
        text-align: right;
    }
    .sa-add-question .sa-add-question-form {
        padding: 20px 25px;
    }
    .sa-add-question .sa-add-question-form .sa-ans {
        margin-bottom: 15px;
    }
    .sa-add-question .sa-add-question-form .sa-ans label {
        display: inline-block;
        width: 10%;
        vertical-align: top;
        font-weight: bold;
        font-size: 12px;
        line-height: 1em;
        margin-bottom: 5px;
    }
    .sa-add-question .sa-add-question-form .sa-ans select {
        display: inline-block;
        width: 89%;
        vertical-align: middle;
        border: 1px solid #ecddf1;
        background: #ecddf1;
        line-height: 1em;
        padding: 10px 20px;
        border-radius: 5px;
        color: #4c5f99;
        font-size: 12px;
        font-weight: bold;
        appearance: auto;
    }
    .sa-add-question .sa-add-question-form .sa-ans textarea {
        display: inline-block;
        width: 89%;
        vertical-align: middle;
        border: 1px solid #ecddf1;
        background: #ecddf1;
        line-height: 1em;
        padding: 10px 20px;
        border-radius: 5px;
        color: #4c5f99;
        font-size: 12px;
        font-weight: bold;
        height: 90px;
    }
    .sa-add-question .sa-add-question-form .sa-add-ops {
        text-align: right;
    }
    .sa-add-question .sa-add-question-form button.savebutton {
        background: #fe608b;
        background: linear-gradient(137deg, #fe608b 0%, #ff99a2 100%);
    }
    .dsaveans {
        display: inline-block;
        vertical-align: middle;
        width: 50%;
        text-align: right;
        padding: 10px 20px;
    }
    .dsaveans button {
        background: #69e465;
        background: linear-gradient(137deg, #69e465 0%, #118c00 100%);
        color: #fff;
        line-height: 1em;
        font-size: 11px;
        padding: 10px 20px;
        border-radius: 30px;
    }
    .daddanswer {
        display: inline-block;
        vertical-align: middle;
        width: 49%;
        
        padding: 10px 20px;
    }
    .daddanswer button {
        background: #fe608b;
        background: linear-gradient(137deg, #fe608b 0%, #ff99a2 100%);
        color: #fff;
        line-height: 1em;
        font-size: 11px;
        padding: 10px 20px;
        border-radius: 30px;
    }
    .derrorpart {
        text-align: right;
        color: red;
        margin-top: 5px;
    }
    .sa-segment-item {
        width: 48%;
        display: inline-block;
        margin: 0 1% 2%;
        background: #4d63a590;
        border-radius: 5px;
        padding: 10px 20px;
        color: #fff;
        cursor: pointer;
    }
    .sa-segment-item h4 {
        font-size: 13px;
        text-transform: capitalize;
        line-height: 1.3em;
    }
    .sa-button {
        text-align: right;
    }

</style>