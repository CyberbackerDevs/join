<template>
    <div>
        <button class="add_interview_details" v-on:click="openInterviewModal">Add Interview Details</button>
        <v-dialog v-model="openInitialInterview" width="800">
            <v-card>
                <h2 class="tpi-add-task-title">On Demand Interview Form</h2>
                <v-card-text>
                    <div class="d-background-holder">
                        <div class="d-form-item">
                           <v-text-field v-model="initialValues.fblink" label="Facebook link"></v-text-field>
                       </div>
                       <div class="d-form-item">
                           <v-text-field v-model="initialValues.interested_post" label="Which post are you most interested in?"></v-text-field>
                       </div>
                       <div class="d-form-item">
                           <v-select
                                v-model="initialValues.is_fulltime"
                                :items="['Full Time', 'Part time']"
                                label="Are you applying for a part-time of full time post?"
                                dense
                            ></v-select>
                       </div>
                       <div class="d-form-item">
                           <v-text-field v-model="initialValues.long_term_career" label="What are your long term career goal?"></v-text-field>
                       </div>
                       <div class="d-form-item">
                            <v-switch
                                v-model="initialValues.currently_employed"
                                inset
                                :label="`Are you currently employed? ${initialValues.currently_employed.toString()}`"
                             ></v-switch>
                       </div>
                       <div class="d-form-item">
                           <v-switch
                                v-model="initialValues.upcomming_medical_issue"
                                inset
                                :label="`Are there any upcoming medical surgeries, pregnancies, or medical procedures that would prevent you from attending work during expected work times for the next 6 months? ${initialValues.upcomming_medical_issue.toString()}`"
                             ></v-switch>
                       </div>
                       <div class="d-form-item">
                           <v-switch
                                v-model="initialValues.travel_plans"
                                inset
                                :label="`Do you have any travel plans that would prevent you from attending work during expected work times for the next 6 months? ${initialValues.travel_plans.toString()}`"
                             ></v-switch>
                       </div>
                       <div class="d-form-item">
                           <v-text-field v-model="initialValues.other_challenges_that_would_prevent_working" label="Are there any other challenges that would keep you from attending work during expected work times for the next 6 months?"></v-text-field>
                       </div>
                       <div class="d-form-item">
                           <v-text-field v-model="initialValues.current_env" label="Can you describe your current work environment?"></v-text-field>
                       </div>
                       <div class="d-form-item">
                           <v-text-field v-model="initialValues.where_are_you_from" label="Where are you from? (country, state/province/ city)"></v-text-field>
                       </div>
                       <div class="d-form-item">
                           <v-switch
                                v-model="initialValues.obtain_nbi"
                                inset
                                :label="`If hired would you be able to obtain NBI clearance in the first 30 days after beginning work? ${initialValues.obtain_nbi.toString()}`"
                             ></v-switch>
                       </div>
                       <div class="d-form-item">
                           <v-select
                                v-model="initialValues.salary_expectations"
                                :items="['$100- $400 per month', '$400 - $600 per month', '$601-$800 per month', '$801 - $1,000 per month', '$1,001 or more']"
                                label="What is your salary expectation?"
                                dense
                            ></v-select>
                       </div>
                       <div class="d-form-item">
                           <v-switch
                                v-model="initialValues.own_noice_canceling_headset"
                                inset
                                :label="`Owns a noise-cancellation headset ${initialValues.own_noice_canceling_headset.toString()}`"
                             ></v-switch>
                       </div>
                       <div class="d-form-item">
                           <v-text-field v-model="initialValues.primary_isp_and_speed" label="Primary Internet Connection and speed"></v-text-field>
                       </div>
                       <div class="d-form-item">
                           <v-text-field v-model="initialValues.secondary_isp_and_speed" label="Back up internet connection and speed"></v-text-field>
                       </div>
                       <div class="d-form-item">
                           <v-select
                                v-model="initialValues.primary_pc_age"
                                :items="['-1', '2', '3', '4', '5', '6', '7', '8', '9', '10+']"
                                label="Age of primary computer"
                                dense
                            ></v-select>
                       </div>
                       <div class="d-form-item">
                           <v-select
                                v-model="initialValues.primary_pc_type"
                                :items="['PC', 'MAC', 'Laptop']"
                                label="PC or MAC primary computer"
                                dense
                            ></v-select>
                       </div>
                       <div class="d-form-item">
                           <v-text-field v-model="initialValues.primary_os_type" label="Operating software on primary computer" hint="(example Windows 10, Windows 8, macOS 10.14: Mojave, etc.)"></v-text-field>
                       </div>
                       <div class="d-form-item">
                           <v-select
                                v-model="initialValues.secondary_pc_type"
                                :items="['PC', 'MAC', 'Laptop']"
                                label="PC or MAC secondary computer"
                                dense
                            ></v-select>
                       </div>
                       <div class="d-form-item">
                            <v-text-field v-model="initialValues.secondary_os_type" label="Operating software on secondary computer" hint="(example Windows 10, Windows 8, macOS 10.14: Mojave, etc.)"></v-text-field>
                       </div>
                       <div class="d-form-item d-interview-sliders">
                            <v-slider
                                v-model="initialValues.english_speaking_ability"
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
                                v-model="initialValues.presense_on_webcam"
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
                                v-model="initialValues.interview_presentation"
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
                                v-model="initialValues.connection_quality"
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
                                v-model="initialValues.distruptions_distractions"
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
                           <v-text-field v-model="initialValues.reasons_to_hire" label="Reasons we should hire this candidate?"></v-text-field>
                       </div>
                       <div class="d-form-item">
                           <v-text-field v-model="initialValues.reasons_not_to_hire" label="Reasons we should NOT hire this candidate?"></v-text-field>
                       </div>
                       <div class="d-form-item">
                           <v-text-field v-model="initialValues.should_proceed_with_kpa" label="Do you believe this person should proceed in the hiring process by having them take a KPA?"></v-text-field>
                       </div>
                       <div class="d-form-item">
                           <v-text-field v-model="initialValues.who_referred_you" label="Who referred you to Cyberbacker?"></v-text-field>
                       </div>
                    </div>
                </v-card-text>

                <v-divider></v-divider>
                <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn v-if="!interviewhowLoading" color="error" text v-on:click="closeInterviewValues">Cancel</v-btn>
                    <v-btn v-if="!interviewhowLoading" color="info" text v-on:click="saveInterviewValues">Save</v-btn>
                    <div class="d-loading-side" v-if="interviewhowLoading"><v-progress-circular indeterminate color="primary"></v-progress-circular></div>
                </v-card-actions>
            </v-card>
        </v-dialog>
    </div>
</template>

<script>
export default {
    name: 'initialinterview',
    props: [
        'info',
    ],
    data(){
        return {
            interviewhowLoading: false,
            openInitialInterview: false,
            initialValues: {
                'fblink': this.info.social,
                'interested_post': '',
                'is_fulltime': 'Full Time',
                'long_term_career': '',
                'currently_employed': false,
                'upcomming_medical_issue': false,
                'travel_plans': false,
                'other_challenges_that_would_prevent_working': '',
                'current_env': '',
                'where_are_you_from': '',
                'obtain_nbi': false,
                'salary_expectations': '$400 - $600 per month',
                'own_noice_canceling_headset': false,
                'primary_isp_and_speed': '',
                'secondary_isp_and_speed': '',
                'primary_pc_age': '2',
                'primary_pc_type': 'PC',
                'primary_os_type': '',
                'secondary_pc_type': 'PC',
                'secondary_os_type': '',
                'english_speaking_ability': 5,
                'presense_on_webcam': 5,
                'interview_presentation': 5,
                'connection_quality': 5,
                'distruptions_distractions': 5,
                'reasons_to_hire': '',
                'reasons_not_to_hire': '',
                'should_proceed_with_kpa': '',
                'who_referred_you': '',
            }
        }
    },
    methods: {
        openInterviewModal(){
            this.openInitialInterview = true;
        },
        closeInterviewValues(){
            this.openInitialInterview = false;
        },
        saveInterviewValues(){
            // console.log('to save data -> ', this.initialValues);
            // console.log('user id -> ', this.info);
            let loggedInUser = localStorage.getItem('userId');

            // console.log('login user -> ', loggedInUser);
            this.initialValues.user_id = this.info.userid;
            this.initialValues.last_touch = loggedInUser;

            this.interviewhowLoading = true;

            const headers = {
                'Access-Control-Allow-Origin': '*'
            }

            // build data
            let interview_data = this.initialValues;

            console.log('background data -> ', interview_data);

            this.$axios.post('https://be2.applytocyberbacker.com/api/users/interview/values', interview_data, { headers: headers })
            .then(function (response) {
                console.log('info -> ', response);
                
                window.location.reload()
            })
            .catch(function (error) {
                console.log('show error ->', error);
            });
        },
        getInterviewDerails(){
            let self = this;

            const headers = {
                'Access-Control-Allow-Origin': '*',
            }

            this.$axios.get('https://be2.applytocyberbacker.com/api/users/interview/values/'+this.info.userid, { headers: headers }, {}, )
            .then(function (response) {
                console.log('interview form info -> ', response.data.data);
                if(Object.keys(response.data.data).length > 0){
                    console.log('has data in interview form');
                    self.initialValues = response.data.data;
                    self.initialValues.currently_employed = (self.initialValues.currently_employed == 1 ? true : false)
                    self.initialValues.upcomming_medical_issue = (self.initialValues.upcomming_medical_issue == 1 ? true : false)
                    self.initialValues.travel_plans = (self.initialValues.travel_plans == 1 ? true : false)
                    self.initialValues.obtain_nbi = (self.initialValues.obtain_nbi == 1 ? true : false)
                    self.initialValues.own_noice_canceling_headset = (self.initialValues.own_noice_canceling_headset == 1 ? true : false)
                    self.initialValues.fblink = self.info.social
                }
                
            })
            .catch(function (error) {
                console.log(error);
            });
        }
    },
    mounted(){
        console.log('assessment info ->', this.info.social);
        this.getInterviewDerails();
        
    }
}
</script>

<style scoped>
    .add_interview_details {
        background: #4d64a7;
        background: linear-gradient(137deg, #485b96 0%, #4d64a7 100%);
        width: 100%;
        display: block;
        color: #fff;
        line-height: 1em;
        padding: 10px 20px;
        border-radius: 30px;
    }
    h2.tpi-add-task-title{
        background: url('~/assets/images/placeheader.jpg') no-repeat center right;
        font-size: 16px;
        line-height: 1em;
        font-weight: 100;
        padding: 15px 25px;
        color: #fff;
        margin-bottom: 15px;
    }
    .d-form-item {
        margin-bottom: 20px;
    }
    .d-interview-sliders {
        margin-bottom: 40px;
    }
    .d-form-item label {
        display: block;
        line-height: 1.2em;
        margin-bottom: 5px;
    }
    .d-form-item input {
        display: block;
        border: 1px solid #ccc;
        width: 100%;
        border-radius: 5px;
        padding: 5px 15px;
    }
    .d-form-item .note {
        font-size: 12px;
        font-weight: bold;
    }
    
</style>