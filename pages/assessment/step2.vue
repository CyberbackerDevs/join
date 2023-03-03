<template>
    <div class="step_one">
        <v-app fluid>
            <form  @submit.prevent="submit">
                <div class="dformitem">
                    <h2>Shapes</h2>
                    <div class="subheads">Arrange the symbols that speaks to you the most:*</div>
                    <div class="dforms">
                        <div class="showshapes">
                            <div class="ditemlabel dlabelleft"></div>
                            <draggable v-model="shapes" ghost-class="ghost" @end="onEnd">
                                <transition-group type="transition" name="flip-list">
                                    <div class="sortable" :id="itemx.id" v-for="itemx in shapes" :key="itemx.id">
                                        <!-- <strong>{{ itemx.name }}</strong> -->
                                        <v-img :src="require('@/assets/images/pic'+itemx.id+'.jpg')" alt=""></v-img>
                                    </div>
                                </transition-group>
                            </draggable>
                        </div>
                    </div>
                </div>
                <div class="dformitem">
                    <h2>Senses</h2>
                    <div class="subheads">Answer each question according to the way you personally feel.<br />Check the box if it is at least somewhat true for you and leave unchecked if it is not very true or not at all true for you.</div>
                    <div class="dforms">
                        <div class="dpositionitem" v-for="(positon, index) in senses" v-bind:key="positon">
                            <v-checkbox
                                v-model="selectedSenses"
                                :label="positon"
                                :value="positon"
                            ></v-checkbox>
                        </div>
                    </div>
                </div>
                <div class="dsubmitforms">
                    <div class="subs" v-if="!showLoading">
                        <button @click="submitStepTwo('Lead')">Save and come back later</button>
                        <button @click="submitStepTwo('ForYAssessment')">Proceed to Y-assessment</button>
                    </div>
                    <div class="dshowloading" v-if="showLoading">
                        <v-progress-circular
                        indeterminate
                        color="primary"
                        ></v-progress-circular>
                    </div>
                    <div class="dshowapplicationerror" v-if="showProceedError">
                        <div class="dmaintitle">Something went wrong, please try again in a few minutes</div>
                        <div class="dsubmain">It maybe due to your slow intenet slow internet or machine compatibility, you can reload the page or try sending the form again</div>
                    </div>
                </div>
            </form>
        </v-app>
    </div>
</template>

<script>
    import draggable from 'vuedraggable'
    import { print } from 'graphql';
    import gql from 'graphql-tag';

    export default {
        head: {
            title: 'Application Process Step 2',
        },
        name: 'step_two',
        layout: 'StageOne',
        components: {
            draggable,
        },
        data(){
            return {
                showProceedError: false,
                showLoading: false,
                localUserid: '',
                hasUserID: false,
                selectedSenses: [],
                shapes: [
                    {
                        id: 1,
                        name: '@/assets/images/shapes/pic1.jpg',
                        dshape: 'square'
                    },
                    {
                        id: 2,
                        name: '@/assets/images/shapes/pic2.jpg',
                        dshape: 'triangle'
                    },
                    {
                        id: 3,
                        name: '@/assets/images/shapes/pic3.jpg',
                        dshape: 'circle'
                    },
                    {
                        id: 4,
                        name: '@/assets/images/shapes/pic4.jpg',
                        dshape: 'asterisk'
                    },
                    {
                        id: 5,
                        name: '@/assets/images/shapes/pic5.jpg',
                        dshape: 'rectangle'
                    },
                    {
                        id: 6,
                        name: '@/assets/images/shapes/pic6.jpg',
                        dshape: 'x'
                    },
                    {
                        id: 7,
                        name: '@/assets/images/shapes/pic7.jpg',
                        dshape: 'dots'
                    },
                    {
                        id: 8,
                        name: '@/assets/images/shapes/pic8.jpg',
                        dshape: 'three'
                    },
                ],
                senses: [
                    'I am easily overwhelmed by strong sensory input.',
                    'I seem to be aware of subtleties in my environment.',
                    'Other people’s moods affect me.',
                    'I tend to be very sensitive to pain.',
                    'I find myself needing to withdraw during busy days,into bed or into a darkened room or any place where I can have some privacy and relief from stimulation.',
                    'I am particularly sensitive to the effects of caffeine',
                    'I am easily overwhelmed by things like bright lights, strong smells,coarse fabrics,or sirens close by.',
                    'I have a rich,complex inner life.',
                    'I am made uncomfortable by loud noises.',
                    'I am deeply moved by the arts or music.',
                    'My nervous system sometimes feels so frazzled that I just have to go off by myself.',
                    'I am conscientious.',
                    'I startle easily.',
                    'I get rattled when I have a lot to do in a short amount of time.',
                    'When people are uncomfortable in a physical environment I tend to know what needs to be done to make it more comfortable (like changing the lighting or the seating).',
                    'I am annoyed when people try to get me to do too many things at once.',
                    'I try hard to avoid making mistakes or forgetting things.',
                    'I make a point to avoid violent movies and TV shows.',
                    'I become unpleasantly aroused when a lot is going on around me.',
                    'Being very hungry creates a strong reaction in me,disrupting my concentration or mood.',
                    'Changes in my life shake me up.',
                    'I notice and enjoy delicate or fine scents, tastes, sounds, works of art..',
                    'I find it unpleasant to have a lot going on at once.',
                    'I make it a high priority to arrange my life to avoid upsetting or overwhelming situations.',
                    'I am bothered by intense stimuli, like loud noises or chaotic scenes.',
                    'When I must compete or be observed while performing a task, I become so nervous or shaky that I do much worse than I would otherwise.',
                    'When I was a child, my parents or teachers seemed to see me as sensitive or shy.'
                ]
            }
        },
        methods: {
            submit(){
                
                // if(this.validateEmailExist(this.form.email_address)){
                //     this.emailIssueDialog = true;
                // }
            },
            validateEmailExist(userEmail){
                if(userEmail == "sample@email.com"){
                    return true;
                }

                return false;
                
            },
            saveandgoback(){
                console.log('message: ', 'save and come back again');
                console.log('shapes ->', this.shapes);
                console.log('senses ->', this.selectedSenses);
            },
            toyassessment(){
                // console.log('go to yassessment');
                this.$router.push('/assessment/step3')
            },
            submitStepTwo(signstatus){
                // console.log('dstatus', status);
                this.showLoading = true;
                let self = this;
                let userid = this.localUserid;

                const headers = {
                    'Access-Control-Allow-Origin': '*',
                    'token': userid
                }

                let dshapenames = [];
                this.shapes.forEach((item, index) => {
                    dshapenames[item.dshape] = ""+index;
                });

                let params = {
                    "shapes":{
                        "square": dshapenames['square'],
                        "triangle": dshapenames['triangle'],
                        "circle": dshapenames['circle'],
                        "asterisk": dshapenames['asterisk'],
                        "rectangle": dshapenames['rectangle'],
                        "x": dshapenames['x'],
                        "dots": dshapenames['dots'],
                        "three": dshapenames['three']
                    },
                    "senses": this.selectedSenses,
                    "status": "ForYAssessment"
                }
                


                this.$axios.post('https://be2.applytocyberbacker.com/api/assessment/step/2', params, { headers: headers})
                .then(function (response) {
                    console.log('info -> ', response);
                    console.log('duser -> ', userid);
                    if(signstatus == "ForYAssessment"){
                        self.$router.push('/assessment/step3?uuid='+userid)
                    } else {
                        self.$router.push('/')
                    }
                })
                .catch(function (error) {
                    console.log('show error ->', error);

                    self.showProceedError = true;
                    self.showLoading = false;
                });
            },
            onEnd(evt){
                console.log(this.shapes);
                this.oldIndex = evt.oldIndex;
                this.newIndex = evt.newIndex;
            },
            showlocal(){
                console.log('applicant info ->', localStorage.getItem('applicant_info'));
            },
            setUserId(){ // building user id 
                let clientinfo = localStorage.getItem('applicant_info');
                clientinfo = JSON.parse(clientinfo);
                
                
                let durluserid = this.$route.query.uuid;
                let useridinfo = '';
                if(typeof(clientinfo) === `undefined` || clientinfo === null){
                    // console.log('no userid in url');
                    if(typeof(durluserid) === `undefined` || durluserid === null){
                        window.location.replace("/assessment/step1");
                    }
                    useridinfo = durluserid;

                    // console.log('fix here -> ', useridinfo);
                    
                } else {
                    // console.log('with userid');
                    useridinfo = clientinfo.api_token;
                }

                

                this.localUserid = useridinfo;
            }
        },
        mounted(){
            this.showlocal();
            this.setUserId();
        }
    }
</script>

<style scoped>
    .dpositionitem {
        width: 49%;
        display: inline-block;
    }

    .sortable {
        width: 12.13%;
        display: inline-block;
    }

    .showshapes {
        text-align: center;
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
        line-height: 1.3em;
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
</style>