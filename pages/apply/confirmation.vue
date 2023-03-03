<template>
    <v-app>
        <div class="d-start-appliation" >
            <ApplyStepper step="4"/>
            <div class="d-start-header">
                <h1>Congratulations!</h1>
            </div>
            <div class="page-content">
                <p>You have completed the application form.</p>
                <p>The next step is to have your initial interview. The interview will be conducted on a first-come, first-served basis.</p>
                <!-- <div class="interview-requirements">
                    <div class="ir-header">
                        PLEASE MAKE SURE YOU HAVE THE FOLLOWING
                    </div>
                    <div class="ir-content">
                        <ul>
                            <li>Webcam turned on and available</li>
                            <li>Headset plugged in and ready to go</li>
                            <li>On a computer and not a mobile device!</li>
                            <li>Stable internet connection you would be using for work</li>
                            <li>In work station that will be used during working hours</li>
                        </ul>
                    </div> 
                </div> -->
                <p>We are now conducting mobile phone applications but for INITIAL INTERVIEWS ONLY.</p>
                <!-- <p>See you now on our On Demand Interview Zoom!</p> -->
                <div class="d-interview-button">
                    <button @click="ProceedWithInitialInterview">Proceed with the initial interview</button>
                </div>
            </div>
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
        }
    },
    methods: {
        ProceedWithInitialInterview(){
            console.log('go to initial interview');
            window.location.href = 'https://cyberbacker.zoom.us/j/977857097'
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

    .ir-header {
        font-size: 21px;
        background: #272727;
        color: #fff;
        padding: 10px 30px;
    }

    .interview-requirements {
        border: 1px solid #272727;
        border-radius: 5px;
        margin-bottom: 25px;
    }

    .ir-content {
        padding: 15px 25px;
    }

    .d-interview-button button {
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