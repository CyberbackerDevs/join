<template>
    <div v-if="Object.keys(interviewDetails).length > 0" class="d-interview-details-holder">
        <h2 class="valassest" v-on:click="showInterviewData">On Demand Interview Details</h2>
        <div class="d-ondemail-info" v-if="showDetails">
            <div class="d-background-holder">
                <div class="d-form-item">
                    <div class="d-question">Facebook link</div>
                    <div class="d-answer">{{interviewDetails.fblink}}</div>
                </div>
                <div class="d-form-item">
                    <div class="d-question">Which post are you most interested in?</div>
                    <div class="d-answer">{{interviewDetails.interested_post}}</div>
                </div>
                <div class="d-form-item">
                    <div class="d-question">Are you applying for a part-time of full time post?</div>
                    <div class="d-answer">{{interviewDetails.is_fulltime}}</div>
                </div>
                <div class="d-form-item">
                    <div class="d-question">What are your long term career goal?</div>
                    <div class="d-answer">{{interviewDetails.long_term_career}}</div>
                </div>
                <div class="d-form-item">
                    <v-switch
                        class="d-small-switch"
                        :v-model="(interviewDetails.currently_employed == 1 ? true : false)"
                        readonly
                        dense
                        inset
                        :label="`Are you currently employed?`"
                        ></v-switch>
                </div>
                <div class="d-form-item">
                    <v-switch
                        class="d-small-switch"
                        v-model="interviewDetails.upcomming_medical_issue"
                        readonly
                        dense
                        inset
                        :label="`Are there any upcoming medical surgeries, pregnancies, or medical procedures that would prevent you from attending work during expected work times for the next 6 months?`"
                        ></v-switch>
                </div>
                <div class="d-form-item">
                    <v-switch
                        class="d-small-switch"
                        v-model="interviewDetails.travel_plans"
                        readonly
                        dense
                        inset
                        :label="`Do you have any travel plans that would prevent you from attending work during expected work times for the next 6 months?`"
                        ></v-switch>
                </div>
                <div class="d-form-item">
                    <div class="d-question">Are there any other challenges that would keep you from attending work during expected work times for the next 6 months?</div>
                    <div class="d-answer">{{interviewDetails.other_challenges_that_would_prevent_working}}</div>
                </div>
                <div class="d-form-item">
                    <div class="d-question">Can you describe your current work environment?</div>
                    <div class="d-answer">{{interviewDetails.current_env}}</div>
                </div>
                <div class="d-form-item">
                    <div class="d-question">Where are you from? (country, state/province/ city)</div>
                    <div class="d-answer">{{interviewDetails.where_are_you_from}}</div>
                </div>
                <div class="d-form-item">
                    <v-switch
                        class="d-small-switch"
                        v-model="interviewDetails.obtain_nbi"
                        inset
                        readonly
                        :label="`If hired would you be able to obtain NBI clearance in the first 30 days after beginning work? `"
                        ></v-switch>
                </div>
                <div class="d-form-item">
                    <div class="d-question">What is your salary expectation?</div>
                    <div class="d-answer">{{interviewDetails.salary_expectations}}</div>
                </div>
                <div class="d-form-item">
                    <v-switch
                        class="d-small-switch"
                        v-model="interviewDetails.own_noice_canceling_headset"
                        inset
                        readonly
                        :label="`Owns a noise-cancellation headset`"
                        ></v-switch>
                </div>
                <div class="d-form-item">
                    <div class="d-question">Primary Internet Connection and speed</div>
                    <div class="d-answer">{{interviewDetails.primary_isp_and_speed}}</div>
                </div>
                <div class="d-form-item">
                    <div class="d-question">Back up internet connection and speed</div>
                    <div class="d-answer">{{interviewDetails.secondary_isp_and_speed}}</div>
                </div>
                <div class="d-form-item">
                    <div class="d-question">Age of primary computer</div>
                    <div class="d-answer">{{interviewDetails.primary_pc_age}} years old</div>
                </div>
                <div class="d-form-item">
                    <div class="d-question">PC or MAC primary computer</div>
                    <div class="d-answer">{{interviewDetails.primary_pc_type}}</div>
                </div>
                <div class="d-form-item">
                    <div class="d-question">Operating software on primary computer</div>
                    <div class="d-answer">{{interviewDetails.primary_os_type}}</div>
                </div>
                <div class="d-form-item">
                    <div class="d-question">PC or MAC secondary computer</div>
                    <div class="d-answer">{{interviewDetails.secondary_pc_type}}</div>
                </div>
                <div class="d-form-item">
                    <div class="d-question">Operating software on secondary computer</div>
                    <div class="d-answer">{{interviewDetails.secondary_os_type}}</div>
                </div>
                <div class="d-form-item d-interview-sliders">
                    <v-slider
                        v-model="interviewDetails.english_speaking_ability"
                        readonly
                        min="0"
                        max="10"
                        label="Rate their English speaking ability:"
                        hint="1 if Doesn't speak english and 10 if Sounds like they live in the USA right now"
                        thumb-color="primary"
                        thumb-label="always"
                        persistent-hint
                        ticks
                    ></v-slider>
                </div>
                <div class="d-form-item d-interview-sliders">
                    <v-slider
                        v-model="interviewDetails.presense_on_webcam"
                        readonly
                        min="0"
                        max="10"
                        label="Rate their presence on webcam"
                        hint="1 if No webcam available and 10 if Looks Like they are in a professional office"
                        thumb-color="primary"
                        thumb-label="always"
                        persistent-hint
                        ticks
                    ></v-slider>
                </div>
                <div class="d-form-item d-interview-sliders">
                    <v-slider
                        v-model="interviewDetails.interview_presentation"
                        readonly
                        min="0"
                        max="10"
                        label="Interview preparation"
                        hint="1 if Just woke up and 10 if Professional well groomed appearance"
                        thumb-color="primary"
                        thumb-label="always"
                        persistent-hint
                        ticks
                    ></v-slider>
                </div>
                <div class="d-form-item d-interview-sliders">
                    <v-slider
                        v-model="interviewDetails.connection_quality"
                        readonly
                        min="0"
                        max="10"
                        label="Quality of connection"
                        hint="1 if Disconnected or had challenges and 10 if Great connection no lags or problems"
                        thumb-color="primary"
                        thumb-label="always"
                        persistent-hint
                        ticks
                    ></v-slider>
                </div>
                <div class="d-form-item d-interview-sliders">
                    <v-slider
                        v-model="interviewDetails.distruptions_distractions"
                        readonly
                        min="0"
                        max="10"
                        label="Disruptions or distractions"
                        hint="1 if Frequent disruptions and 10 NO disruptions or distractions"
                        thumb-color="primary"
                        thumb-label="always"
                        persistent-hint
                        ticks
                    ></v-slider>
                </div>
                <div class="d-form-item">
                    <div class="d-question">Reasons we should hire this candidate?</div>
                    <div class="d-answer">{{interviewDetails.reasons_to_hire}}</div>
                </div>
                <div class="d-form-item">
                    <div class="d-question">Reasons we should NOT hire this candidate?</div>
                    <div class="d-answer">{{interviewDetails.reasons_not_to_hire}}</div>
                </div>
                <div class="d-form-item">
                    <div class="d-question">Do you believe this person should proceed in the hiring process by having them take a KPA?</div>
                    <div class="d-answer">{{interviewDetails.should_proceed_with_kpa}}</div>
                </div>
                <div class="d-form-item">
                    <div class="d-question">Who referred you to Cyberbacker?</div>
                    <div class="d-answer">{{interviewDetails.who_referred_you}}</div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'interviewDetails',
    props: [
        'info'
    ],
    data(){
        return {
            showDetails: false,
            interviewDetails: {}
        }
    },
    methods: {
        showInterviewData(){
            this.showDetails = !this.showDetails;
        },
        getInterviewDetails(){
            console.log('interview details -> ', this.info.userid);

            let self = this;
            const headers = {
                'Access-Control-Allow-Origin': '*',
            }

            this.$axios.get('https://be2.applytocyberbacker.com/api/users/interview/values/'+this.info.userid, { headers: headers }, {}, )
            .then(function (response) {
                console.log('interview info -> ', response.data.data);
                self.interviewDetails = response.data.data;
                self.interviewDetails.currently_employed = (self.interviewDetails.currently_employed == 1 ? true : false)
                self.interviewDetails.upcomming_medical_issue = (self.interviewDetails.upcomming_medical_issue == 1 ? true : false)
                self.interviewDetails.travel_plans = (self.interviewDetails.travel_plans == 1 ? true : false)
                self.interviewDetails.obtain_nbi = (self.interviewDetails.obtain_nbi == 1 ? true : false)
                self.interviewDetails.own_noice_canceling_headset = (self.interviewDetails.own_noice_canceling_headset == 1 ? true : false)
            })
            .catch(function (error) {
                console.log(error);
            });
        }
    },
    mounted(){
        // console.log('getting info from interview details -> ', this.info);
        this.getInterviewDetails();
    }
}
</script>

<style scoped>
    h2.valassest {
        text-transform: uppercase;
        font-weight: 100;
        background: #4c5f99;
        color: #fff;
        text-align: center;
        line-height: 1em;
        padding: 7px;
        letter-spacing: 1px;
        margin-bottom: 15px;
        cursor: pointer;
    }
    .d-form-item {
        margin-bottom: 20px;
    }
    .d-interview-sliders {
        margin-bottom: 40px;
    }
    .d-small-switch {
        font-size: 13px;
    }
    .d-background-holder {
        padding: 0 15px;
    }
    .d-form-item .d-question {
        font-size: 13px;
        line-height: 1.2em;
        margin-bottom: 5px;
        font-weight: bold;
        color: #666;
    }
    .d-form-item .d-answer {
        background: #696969;
        color: #fff;
        padding: 5px 15px;
        border-radius: 7px;
    }
    .d-interview-details-holder {
        margin-bottom: 60px;
    }
</style>

<style>
    .d-small-switch .v-input__control .v-messages {
        display: none;
    }
    .d-small-switch .v-input__control .v-input__slot label {
        font-size: 13px;
    }
</style>