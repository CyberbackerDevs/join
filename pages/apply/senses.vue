<template>
    <v-app>
        <div class="d-start-appliation" >
            <ApplyStepper step="2"/>
            <div class="d-start-header">
                <h1>Shapes and Senses</h1>
            </div>
            <div class="dformitem">
                <div class="subheads">Arrange the symbols that speaks to you the most from left to right</div>
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
                <div class="subheads">Answer each question according to the way you feel.<br />Check the box if it is at least somewhat true for you and leave unchecked if it is not very true or not at all true for you.</div>
                <div class="dforms">
                    <div class="dpositionitem" v-for="(positon, index) in senses" v-bind:key="index">
                        <v-checkbox
                            v-model="selectedSenses"
                            :label="positon"
                            :value="positon"
                        ></v-checkbox>
                    </div>
                </div>
            </div>
            <div class="subs" v-if="!showLoading">
                <button @click="submitStepTwo('Lead')">Save and come back later</button>
                <button @click="submitStepTwo('ForYAssessment')">Proceed to Y-assessment</button>
            </div>
            <div class="dprogress" v-if="sendingpross">Sending..</div>
        </div>
    </v-app>
</template>

<script>
import draggable from 'vuedraggable'
import { print } from 'graphql';
import gql from 'graphql-tag';

export default {
    head: {
        title: 'Senses Questions',
    },
    name: 'assessment_senses',
    layout: 'Avtwo',
    components: {
        draggable,
    },
    data(){
        return {
            oldIndex: '',
            newIndex: '',
            localUserid: '',
            selectedSenses: [],
            showLoading: false,
            sendingpross: false,
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
                'I am aware of the subtleties in my environment.',
                'Other people’s moods affect me.',
                'I tend to be very sensitive to pain.',
                'I find myself needing to withdraw during busy days, into bed, a darkened room, or any place where I can have some privacy and relief from stimulation.',
                'I am susceptible to the effects of caffeine.',
                'I am easily overwhelmed by things like bright lights, strong smells, coarse fabrics, or sirens close by.',
                'I have a rich and complex inner life.',
                'I feel uncomfortable with loud noises.',
                'The arts and music deeply move me.',
                'My nervous system sometimes feels so frazzled that I must go off alone.',
                'I am conscientious.',
                'I startle easily.',
                'I get rattled when I have a lot to do in a short amount of time.',
                'When people are uncomfortable in a physical environment, I know what to do to make it more comfortable (like changing the lighting or seating).',
                'I am annoyed when people try to get me to do too many things at once.',
                'I try hard to avoid making mistakes or forgetting things.',
                'I make a point to avoid violent movies and TV shows.',
                'I become stirred up when a lot is going on around me.',
                'Being hungry triggers a strong reaction, disrupting my concentration or mood.',
                'Changes in my life shake me up.',
                'I notice and enjoy delicate or refined scents, tastes, sounds, and works of art.',
                'I find it hard to multitask.',
                'I make it a high priority to arrange my life to avoid upsetting or overwhelming situations.',
                'I am bothered by intense stimuli, like loud noises or chaotic scenes.',
                'When I must compete or be observed while performing a task, I become so nervous or shaky that I do much worse than I would otherwise.',
                'When I was a child, my parents or teachers tended to see me as sensitive or shy.'
            ]
        }
    },
    methods: {
        onEnd(evt){
            // console.log(this.myArray);
            this.oldIndex = evt.oldIndex;
            this.newIndex = evt.newIndex;
            this.hasElementReArranged = true;
        },
        submitStepTwo(signstatus){
            // console.log('dstatus', status);
            this.showLoading = true;
            this.sendingpross = true;
            let self = this;
            let userid = this.localUserid;
            
            if(this.localUserid == ''){
                return false;
            }

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
                    self.$router.push('/apply/assessment?uuid='+userid)
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
        setupUserID(){
            console.log('this id ->', this.$route.query.uuid);
            this.localUserid = this.$route.query.uuid;
        },
    },
    mounted(){
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
        /* font-family: 'Special Elite'; */
        text-shadow: 4px 4px 8px #ccc;
        font-family: 'Arimo', sans-serif;
        letter-spacing: 2px;
        font-size: 75px;
        line-height: 1em;
    }
    .showshapes .sortable {
        width: 12.13%;
        display: inline-block;
    }

    .dformitem .subheads {
        margin-bottom: 15px;
        font-size: 14px;
    }

    .dformitem {
        margin-bottom: 50px;
    }

    .subs button {
        display: inline-block;
        margin: 30px auto;
        padding: 15px 60px;
        background: #f5f7fd;
        border-radius: 10px;
        transition: all 0.5s ease;
        box-shadow: 0 0 12px #c7c7c7;
        cursor: pointer;
        text-decoration: none;
        margin-right: 10px;
    }
</style>

<style>
    body {
        background: #fff !important;
    }
    .dpositionitem .v-messages {
        display: none !important;
    }
    .dpositionitem .v-input--selection-controls {
        margin-top: 0 !important;
    }
    
</style>