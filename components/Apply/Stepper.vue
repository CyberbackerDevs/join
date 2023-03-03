<template>
    <div>
        <v-stepper flat :value="step">
            <v-stepper-header>
                <v-stepper-step :complete="step > 1" step="1">
                    Applicant Profile
                </v-stepper-step>

                <v-divider></v-divider>

                <v-stepper-step :complete="(step > 2)" step="2">
                    Shape & Senses
                </v-stepper-step>

                <v-divider></v-divider>

                <v-stepper-step :complete="(step > 3)" step="3">
                    Assessment
                </v-stepper-step>

                <v-divider></v-divider>

                <v-stepper-step step="4">
                    Confirmation
                </v-stepper-step>
            </v-stepper-header>
        </v-stepper>
    </div>
</template>

<script>
export default {
    name: 'assessment_steps',
    props: ['step'],
    data(){
        return {}
    },
}
</script>
