<template>
    <div class="general-settings">
        this is the general settings
    </div>
</template>

<script>
export default {
    name: 'general',
    methods: {
        getAllCareerPeople(){
            console.log('get all career in settings');

            // get all careers
            let self = this;
                
            const headers = {
                'Access-Control-Allow-Origin': '*',
            }

            this.$axios.get('https://be2.applytocyberbacker.com/api/users/career/list', { headers: headers }, {})
            .then(function (response) {
                console.log('people -> ', response.data.data);
                
            })
            .catch(function (error) {
                console.log(error);
            });
        }
    },
    mounted(){
        this.getAllCareerPeople();
    }
}
</script>