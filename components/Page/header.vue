<template>
    <div class="page_header_part">
        <h2>{{title}}</h2>
        <div class="sub-title">{{subtitle}}</div>
    </div>
</template>

<script>
export default {
    name: 'index',
    props: [
        'title',
        'subtitle'
    ],
    data(){
        return {
            
        }
    }
}
</script>

<style scoped>
    .page_header_part h2 {
        line-height: 1em;
        margin-bottom: 5px;
        font-weight: 600;
        font-size: 18px;
    }
    .page_header_part .sub-title {
        font-size: 11px;
    }
    .page_header_part {
        margin-bottom: 30px;
    }
</style>