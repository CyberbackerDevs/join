<template>
    <div class="dash-messages-content">
        <div class="dmc-inner">
            <div class="dtitle">
                <div class="dt-inner">
                    <h3>Messages</h3>
                    <div class="sub-title">messages for you</div>
                </div>
            </div>
            <div class="dmhead">
                <div class="dmh-inner">
                    <div class="s_item s-name">name</div>
                    <div class="s_item s-title">title</div>
                    <div class="s_item s-date">date</div>
                    <div class="s_item s-division">division</div>
                </div>
            </div>
            <div class="dmcontent">
                <div class="dmc-inner">
                    <div class="dmessageitem" v-for="(item, key) in messages" :key="key">
                        <div class="m-item m-name">
                            <div class="dimage">
                                <img src="~/assets/images/prof_icon.png" />
                            </div>
                            <div class="dname">{{ item.name }}</div>
                        </div>
                        <div class="m-item m-title">{{ item.title }}</div>
                        <div class="m-item m-date">{{ item.date }}</div>
                        <div class="m-item m-division">{{ item.division }}</div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>

    export default {
        data () {
            return {
                switch1: true,
                messages: [
                    {
                        name: 'John Doe',
                        title: 'issues fro some',
                        date: 'January 1, 2021',
                        division: 'Growth'
                    },
                    {
                        name: 'John Doe',
                        title: 'issues fro some',
                        date: 'January 1, 2021',
                        division: 'Growth'
                    },
                    {
                        name: 'John Doe',
                        title: 'issues fro some',
                        date: 'January 1, 2021',
                        division: 'Growth'
                    },
                    {
                        name: 'John Doe',
                        title: 'issues fro some',
                        date: 'January 1, 2021',
                        division: 'Growth'
                    },
                    {
                        name: 'John Doe',
                        title: 'issues fro some',
                        date: 'January 1, 2021',
                        division: 'Growth'
                    },
                    {
                        name: 'John Doe',
                        title: 'issues fro some',
                        date: 'January 1, 2021',
                        division: 'Growth'
                    },
                    {
                        name: 'John Doe',
                        title: 'issues fro some',
                        date: 'January 1, 2021',
                        division: 'Growth'
                    },
                    {
                        name: 'John Doe',
                        title: 'issues fro some',
                        date: 'January 1, 2021',
                        division: 'Growth'
                    },
                    {
                        name: 'John Doe',
                        title: 'issues fro some',
                        date: 'January 1, 2021',
                        division: 'Growth'
                    },
                ]
            }
        },
    }
</script>

<style scoped>
    .dash-messages-content {
        background: #fff;
        border-radius: 10px;
        box-shadow: 0px 0px 12px rgb(78 101 167 / 45%);
        overflow: hidden;
        /* width: 95%; */
    }
    .dmhead .s_item {
        display: inline-block;
        padding: 10px 15px;
        line-height: 1em;
        font-weight: bold;
        text-transform: uppercase;
        font-size: 12px;
    }
    .dmcontent .m-item {
        display: inline-block;
        padding: 10px 15px;
        line-height: 1em;
        font-size: 11px;
        font-weight: 400;
    }
    .dmhead .s_item.s-name,
    .dmessageitem .m-item.m-name {
        width: 35%;
    }
    .dmhead .s_item.s-title,
    .dmessageitem .m-item.m-title {
        width: 25%;
    }
    .dmhead .s_item.s-date,
    .dmessageitem .m-item.m-date {
        width: 20%;
    }
    .dmhead .s_item.s-division,
    .dmessageitem .m-item.m-division {
        width: 18%;
    }
    .dmh-inner {
        background: #ecddf1;
    }
    .dmessageitem .m-item.m-name .dimage {
        margin-right: 10px;
    }
    .dmessageitem .m-item.m-name .dimage,
    .dmessageitem .m-item.m-name .dname {
        display: inline-block;
        vertical-align: middle;
    }
    .dmessageitem .m-item.m-name .dimage img {
        width: 35px;
        border-radius: 50px;
        border: 2px solid #8092c8;
    }
    .dt-inner {
        padding: 20px 20px 15px;
    }
    .dt-inner h3 {
        line-height: 1em;
        margin-bottom: 3px;
    }
    .dt-inner .sub-title {
        line-height: 1em;
        font-size: 12px;
    }
</style>