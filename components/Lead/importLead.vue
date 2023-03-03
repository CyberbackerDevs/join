<template>
    <div class="dselectedinner">
        <div class="dheadselectedinfo" :style="{ background: 'url(' + require('@/assets/images/placeheader.jpg') + ') center right no-repeat' }">
            <div class="sheadinner">
                <div class="dtitle">
                    <h2>Import Source Leads</h2>
                    <div class="subhead">use only CSV or Excell format</div>
                </div>
                <div class="dsitems sendinvite">
                    <PageAddButton label="Import now"/>
                </div>
            </div>
        </div>
        <div class="reply_based">
            <div class="dformhere">
                <div class="form-item">
                    <label for="">import a file</label>
                    <input type="file" name="sourceleads">
                    <small>file must be either csv or excell format</small>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'importLead',
    props: [
        'info'
    ],
    data(){
        return {
            
        }
    }
}
</script>

<style scoped>
    .dselectedinner {
        border-radius: 5px;
        overflow: hidden;
    }
    .dheadselectedinfo {
        background-size: cover !important;
        padding: 30px 30px 15px;
        color: #fff;
    }
    .dtitle {
        display: inline-block;
        width: 60%;
        vertical-align: top;
    }

    .dsitems.sendinvite {
        display: inline-block;
        width: 39%;
        text-align: right;
        vertical-align: middle;
    }

    .dheadselectedinfo h2 {
        font-size: 26px;
        font-weight: 400 !important;
        line-height: 1em;
        margin-bottom: 5px;
    }

    .subhead {
        margin-bottom: 10px;
        line-height: 1em;
        font-size: 14px;
        font-weight: 100;
    }

    .reply_based {
        background: #fff;
        padding: 15px 30px;
        font-size: 14px;
    }

    .form-item label {
        display: block;
        font-size: 14px;
        line-height: 1em;
        margin-bottom: 10px;
    }

    .form-item input {
        display: block;
    }

    .form-item small {
        font-size: 12px;
        font-style: italic;
    }
    
</style>