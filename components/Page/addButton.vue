<template>
    <button>{{label}}</button>
</template>

<script>
export default {
    name: 'index',
    props: [
        'label'
    ],
    data(){
        return {
            
        }
    }
}
</script>

<style scoped>
    button {
        background: rgb(254,96,139);
        background: linear-gradient(137deg, rgba(254,96,139,1) 0%, rgba(255,153,162,1) 100%);
        color: #fff;
        font-weight: 400;
        padding: 10px 20px;
        border-radius: 26px;
        text-transform: capitalize;
    }
</style>