<template>
    <div class="showchart">
        <!-- <v-sparkline
            :labels="labels"
            :value="value"
            :line-width="width"
            :fill="fill"
            :type="type"
            :auto-line-width="autoLineWidth"
        ></v-sparkline> -->
    </div>
</template>

<script>
export default {
    name: 'index',
    props: [
        'uservalues'
    ],
    data(){
        return {
            width: 10,
            radius: 10,
            padding: 2,
            lineCap: 'round',
            labels: [
                'aesthetic',
                'altruist',
                'economic',
                'individualistic',
                'political',
                'regulatory',
                'theoretical',
            ],
            value: [57, 57, 57, 57, 57, 57, 57],
            fill: false,
            type: 'bar',
            autoLineWidth: false,
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