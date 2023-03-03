    <template>
        <div class="step_one">
            <v-app fluid>
                <!-- <div class="d-fluid-advisory">
                    <p>Please be advised that our server is under maintenance. However, we always ensure to accommodate your applications. You can click on the link below to continue. We apologize for any inconvenience.</p>

                    <a class="dubs-form-link" href="https://forms.gle/sD8P39mUBjgHp39Q9">https://forms.gle/sD8P39mUBjgHp39Q9</a>
                </div> -->
                    <h2>Personal Information</h2>
                    <v-container class="lighten-5 mb-6 dforminfo">
                        <v-row class="mb-6">
                            <v-col>
                                <div class="form-item" ref="firstnamepart">
                                    <label for="">First Name</label>
                                    <input type="text" v-model="form.first_name" required>
                                    <div class="dshowrequired" v-if="first_name_errors_show">First name is required</div>
                                </div>
                            </v-col>
                            <v-col>
                                <div class="form-item" ref="lastnamepart">
                                    <label for="">Last Name</label>
                                    <input type="text" v-model="form.last_name" required>
                                    <div class="dshowrequired" v-if="last_name_errors_show">Last name is required</div>
                                </div>
                            </v-col>
                        </v-row>
                        <v-row class="mb-6">
                            <v-col>
                                <div class="form-item" ref="mobilepart">
                                    <label for="">Mobile number</label>
                                    <input type="text" v-model="form.mobile_number" required>
                                    <div class="dshowrequired" v-if="mobile_errors_show">Mobile number is required</div>
                                </div>
                            </v-col>
                            <v-col>
                                <div class="form-item" ref="emailpart">
                                    <label for="">Email Address</label>
                                    <input type="text" v-model="form.email_address" required>
                                    <div class="dshowrequired" v-if="email_errors_show">Email must be a valid email address</div>  
                                </div>
                            </v-col>
                        </v-row>
                        <v-row>
                            <v-col>
                                <div class="form-item" ref="birthdatepart">
                                    <v-menu
                                        v-model="showPicker"
                                        :close-on-content-click="false"
                                        transition="scale-transition"
                                        max-width="290px"
                                        min-width="290px"
                                    >
                                        <template v-slot:activator="{ on }">
                                            <label for="">Birthdate</label>
                                            <input type="text" v-model="form.date_of_birth" persistent-hint readonly v-on="on" required placeholder="Click me">
                                        </template>
                                        <v-date-picker
                                            v-model="form.date_of_birth"
                                            no-title
                                            @input="showPicker = false"
                                            :show-current="true"
                                        ></v-date-picker>
                                    </v-menu>
                                    <div class="dshowrequired" v-if="showForBirthdate">Birthdate is required</div>
                                </div>
                            </v-col>
                            <v-col>
                                <div class="form-item" ref="addresspart">
                                    <label for="">Address</label>
                                    <input type="text" v-model="form.home_address">
                                    <div class="dshowrequired" v-if="address_errors_show">Address is required</div>
                                </div>
                            </v-col>
                        </v-row>
                        <v-row>
                            <v-col>
                                <div class="form-item">
                                    <label for="">Bio</label>
                                    <textarea name="" id="" cols="30" rows="10" v-model="form.bio"></textarea>
                                </div>
                            </v-col>
                        </v-row>
                    </v-container>
                    <h2 ref="uploadcvresume">Upload Your Resume/CV:</h2>
                    <div class="subheads">resume should be in PDF format and is following this naming format - Last Name, First Name, Middle Name</div>
                    <div class="dshowrequired" v-if="showforuploadcv">Please add a Resume/CV</div>
                    <div class="dshowrequired" v-if="showUploadResumeFailed">Upload failed. its either the file format us not .pdf or the file size exceeds 2MB</div>
                    <div class="dformsections">
                        
                        <div class="form-item">
                            <input type="file" id="file" ref="file" v-on:change="uploadResume()"/>
                        </div>
                    </div>
                    <h2 ref="socialpart">Social Media Profiles</h2>
                    <div class="subheads">Add active social media profile links</div>
                    <div class="dformsections">
                        
                        <div class="form-item">
                            <label for="">Facebook Link</label>
                            <input type="text" v-model="form.facebook">
                            <div class="dshowrequired" v-if="showsocialmediarequired">Facebook link is required</div>
                        </div>
                    </div>

                    <h2 ref="positionpart">Position That You're Applying For*</h2>
                    <div class="dshowrequired" v-if="showpositionisrequired">Please select a position you wish to apply</div>
                    <div class="subheads">Please select the positions you wish to apply</div>
                    
                    <div class="dformsections">
                        
                        <div class="dforms">
                            <div class="dpositionitem" v-for="(positon, index) in positions" v-bind:key="index">
                                <v-checkbox
                                    v-model="form.positions"
                                    :label="positon.variation_value"
                                    :value="positon.variation_value"
                                ></v-checkbox>
                            </div>
                        </div>
                    </div>
                    

                    <h2 ref="otherjobs">Other online jobs:</h2>
                    <div class="dshowrequired" v-if="showoutstandingjob">We need to know if you have an outstanding job</div>
                    <div class="subheads">Are you currently employed?</div>
                    <div class="dformsections">
                        
                        <div class="dotherjobs">
                            <div class="dradiobots">
                                <v-radio-group v-model="form.otherjobs" column>
                                    <v-radio
                                        label="Yes"
                                        value="Yes"
                                    ></v-radio>
                                    <v-radio
                                        label="No"
                                        value="No"
                                    ></v-radio>
                                </v-radio-group>
                            </div>
                        </div>
                    </div>
                    

                    <h2>Where Did You Hear About Us?*</h2>
                    <div class="subheads"></div>
                    <div class="dformsections">
                        
                        <div class="dforms">
                            <v-radio-group v-model="form.hearfrom" row>
                                <v-radio
                                    class="dpositionitem"
                                    v-for="(hears, index) in hearFromUs"
                                    v-bind:key="index"
                                    :label="hears"
                                    :value="hears"
                                ></v-radio>
                            </v-radio-group>
                            <div class="dother">
                                <input type="text">
                            </div>
                        </div>
                    </div>
                    

                    

                    <h2>Referred by</h2>
                    <div class="dformsections">
                        
                        <div class="form-item" v-if="!hasReference">
                            <input type="text" v-model="form.referred_by" required>
                        </div>
                        <div class="form-item" v-if="hasReference">
                            <div class="drefvalue">{{form.referred_by}}</div>
                        </div>
                    </div>
                    

                    <div class="dsubmitforms">
                        <div class="subs" v-if="form.cv_link != ''">
                            <button type="submit" @click="submitItem" v-if="!showLoading">Proceed to Application</button>
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
                    </div>
            </v-app>
        </div>
    </template>

    <script>
        import { print } from 'graphql';
        import gql from 'graphql-tag';

        export default {
            head: {
                title: 'Application Process Step 1',
            },
            name: 'step_one',
            layout: 'StageOne',
            data(){
                return {
                    showProceedError: false,
                    showForBirthdate: false,
                    showforuploadcv: false,
                    showpositionisrequired: false,
                    showoutstandingjob: false,
                    showsocialmediarequired: false,
                    showLoading: false,
                    // positions: ["Lead backer", "Transaction Backer", 'Inbound Sales Agent', 'Growth Backer', 'Command Backer', 'Cyberbacker', 'Social Media Backer', 'Cyber Recruiter', 'Graphic Artist', 'Video Editor Backer', 'Client Relations Backer', 'Career Backer', 'Legal Backer', 'QA Backer', 'Listing Backer', 'Agent Backer', 'Concierge Backer', 'Productivity Backer', 'Cyber Secret Shopper', 'Auditor'],
                    positions: [],
                    selectedPosition: [],
                    hearFromUs: ["Facebook", "Instagram", "Twitter", "Onlinejobs.ph", "Craigslist", "Cyberbacker", "LinkedIn", "Youtube", "Tiktok", "Indeed", "Billboard", "Google Search",  "Others"],
                    selectedHears: 'Others',
                    radioOtherJobs: '',
                    form: {
                        first_name: '',
                        middle_name: '',
                        last_name: '',
                        date_of_birth: '',
                        mobile_number: '',
                        email_address: '',
                        home_address: '',
                        facebook: '',
                        referred_by: '',
                        bio: '',
                        positions: [],
                        otherjobs: '',
                        hearfrom: 'Others',
                        cv_link: '',
                        cv_up: ''
                    },
                    cv_link: '',
                    work_experience: [],
                    education: [],
                    referals: [],
                    showPicker: false,
                    selectedDate: null,
                    emailIssueDialog: false,
                    alignments: [
                        'start',
                        'center',
                        'end',
                    ],
                    hasReference: false,
                    email_errors: "",
                    email_errors_show: false,
                    first_name_errors_show: false,
                    last_name_errors_show: false,
                    mobile_errors_show: false,
                    address_errors_show: false,
                    showUploadResumeFailed: false,
                    reg: /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,24}))$/,
                }
            },
            methods: {
                isrefredonly(){
                    return true
                },
                goto(refName) {
                    var element = this.$refs[refName];
                    var top = element.offsetTop;

                    // window.scrollTo(0, top);
                    window.scrollTo({
                        top: top,
                        left: 0,
                        behavior: 'smooth'
                    })
                },
                submitItem(){
                    let self = this;
                    console.log(this.form);

                    this.email_errors_show = false;
                    this.first_name_errors_show = false;
                    this.last_name_errors_show = false;
                    this.mobile_errors_show = false;
                    this.address_errors_show = false;

                    this.showForBirthdate = false;
                    this.showforuploadcv = false;
                    this.showoutstandingjob = false;
                    this.showsocialmediarequired = false;

                    if(this.form.first_name == ""){
                        this.first_name_errors_show = true;
                        this.goto('firstnamepart');
                        return;
                    }

                    if(this.form.last_name == ""){
                        this.last_name_errors_show = true;
                        this.goto('lastnamepart');
                        return;
                    }

                    if(this.form.mobile_number == ""){
                        this.mobile_errors_show = true;
                        this.goto('mobilepart');
                        return;
                    }

                    if(this.form.home_address == ""){
                        this.address_errors_show = true;
                        this.goto('addresspart');
                        return;
                    }

                    // check if email is legit
                    if(!this.reg.test(this.form.email_address)){
                        this.email_errors_show = true;
                        this.goto('emailpart');
                        return;
                    }

                    // check if email is empty
                    if(this.form.email_address == ""){
                        this.email_errors_show = true;
                        this.goto('emailpart');
                        return;
                    }

                    // cehck if birthdate is empty
                    if(this.form.date_of_birth == ""){
                        this.showForBirthdate = true;
                        this.goto('birthdatepart');
                        return;
                    }

                    // check if social media is empty
                    if(this.form.facebook == ""){
                        this.showsocialmediarequired = true;
                        this.goto('socialpart');
                        return;
                    }

                    // check if position has values
                    if(Object.keys(this.form.positions).length === 0){
                        this.showpositionisrequired = true;
                        this.goto('positionpart');
                        return;
                    }

                    

                    // check if other jobs is empty
                    if(this.form.otherjobs == ''){
                        this.showoutstandingjob = true;
                        this.goto('otherjobs');
                        return;
                    }
                    
                    // CY is required
                    if(this.form.cv_link == ""){
                        this.showforuploadcv = true;
                        this.goto('uploadcvresume');
                        return;
                    }

                    // check if bio is empty
                    if(this.form.bio == ""){
                        this.form.bio = 'none';
                    }

                    // refered by must be na if no referral
                    if(this.form.referred_by == undefined){
                        this.form.referred_by = 'n/a';
                    }

                    this.showLoading = true;

                    console.log(this.form);

                    console.log('positions -> ', this.form.positions);

                    const headers = {
                        "Access-Control-Allow-Origin": "*"
                    }

                    let params = {
                        "firstname": this.form.first_name,
                        "lastname": this.form.last_name,
                        "mobile": this.form.mobile_number,
                        "email": this.form.email_address,
                        "birthdate": this.form.date_of_birth,
                        "address": this.form.home_address,
                        "bio": this.form.bio,
                        "social": this.form.facebook,
                        "positions": this.form.positions,
                        "hasotherjobs": "none",
                        "hearfrom": this.form.hearfrom,
                        "resume": this.form.cv_link,
                        "referral": this.form.referred_by
                    }

                    // this.$axios.get('https://be2.applytocyberbacker.com/api/assessment/step/1', { headers: headers, params: params })
                    this.$axios.post('https://be2.applytocyberbacker.com/api/assessment/step/1', params, { headers: headers })
                    .then(function (response) {
                        console.log('info -> ', response);
                        const linfo = JSON.stringify(response.data.data);
                        localStorage.setItem('applicant_info', linfo);
                        
                        // console.log(linfo);
                        self.$router.push('/assessment/step2')
                    })
                    .catch(function (error) {
                        console.log('some error on api');

                        self.showProceedError = true;
                        self.showLoading = false;


                        console.log('error ->', error);
                    });

                    
                },
                uploadResume(){
                    console.log('upload here', this.$refs.file.files[0]);

                    // this.cv_up = this.$refs.file.files[0];

                    let self = this;
                    
                    const headers = {
                        'Access-Control-Allow-Origin': '*',
                    }

                    let formData = new FormData();
                    formData.append('file', this.$refs.file.files[0]);

                    this.$axios.post('https://be2.applytocyberbacker.com/api/resume/upload', formData, { headers: headers })
                    .then(function (response) {
                        console.log('info -> ', response.data.data.resume);
                        self.form.cv_link = "https://be2.applytocyberbacker.com/resume/"+response.data.data.resume;
                        self.showUploadResumeFailed = false;
                        // // console.log('length -> ', Object.keys(response.data.data.email).length);
                        // if(Object.keys(response.data.data.email).length > 0){
                        //     self.email_errors = "Email already registered"
                        //     self.email_errors_show = true
                        // }
                    })
                    .catch(function (error) {
                        console.log(error);
                        self.showUploadResumeFailed = true;

                    });
                },
                requireElement(event){
                    console.log(event.target.value);
                },
                getPositions(){
                    let self = this;
                    
                    const headers = {
                        'Access-Control-Allow-Origin': '*',
                    }

                    this.$axios.get('https://be2.applytocyberbacker.com/api/positions/active', { headers: headers }, {})
                    .then(function (response) {
                        console.log('Positions -> ', response.data.data.positions);
                        self.positions = response.data.data.positions;
                    })
                    .catch(function (error) {
                        console.log(error);
                    });
                },
                checkEmailAvailability(){
                    let self = this;
                    const headers = {
                        'Access-Control-Allow-Origin': '*',
                    }

                    let disemail = this.form.email_address;
                    
                    this.$axios.post('https://be2.applytocyberbacker.com/api/assessment/checkemail', {
                        email: disemail
                    }, { headers: headers })
                    .then(function (response) {
                        console.log('info -> ', response.data.data.email);
                        // console.log('length -> ', Object.keys(response.data.data.email).length);
                        if(Object.keys(response.data.data.email).length > 0){
                            self.email_errors = "Email already registered"
                            self.email_errors_show = true
                        }
                    })
                    .catch(function (error) {
                        console.log(error);
                    });
                },
                checkEmail(){
                    
                    console.log(this.form.email_address);
                    if (this.reg.test(this.form.email_address)) {
                        this.email_errors = ""
                        this.email_errors_show = false
                        console.log('hide errior');

                        // create email availability
                        // this.checkEmailAvailability();

                        
                    } else {
                        // this.msg['email'] = '';
                        this.email_errors = "Please enter a valid email address"
                        this.email_errors_show = true
                        console.log('show errior');
                    }
                },
                getReference(){
                    this.form.referred_by = this.$route.query.ref;
                    console.log('refs ->', this.$route.query.ref);
                    if(this.$route.query.ref != undefined){
                        this.hasReference = true;
                    }
                }
            },
            mounted(){
                this.getPositions();
                this.getReference();
            }
        }
    </script>

    <style scoped>

        .dpositionitem {
            width: 22%;
            display: inline-block;
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
            line-height: 1em;
            margin-bottom: 25px;
        }

        .dsubmitforms button {
            margin-top: 45px;
            background: #0896d8;
            border-bottom: 4px solid #0075aa;
            padding: 15px 30px;
            color: #fff;
            line-height: 1em;
            border-radius: 10px;
        }

        .dubs-form-link {
            margin-top: 45px;
            background: #0896d8;
            border-bottom: 4px solid #0075aa;
            padding: 15px 30px;
            color: #fff;
            line-height: 1em;
            border-radius: 10px;
            text-decoration: none;
        }
        
        .d-fluid-advisory {
            text-align: center;
            padding-top: 50px;
        }

        .d-fluid-advisory p {
            text-align: center;
            font-size: 18px;
            margin-bottom: 45px;
        }

        .v-input--selection-controls {
            margin: 0 !important;
            padding: 0 !important;
        }
        .dformitem {
            margin-bottom: 20px;
        }
        .dformsections {
            padding: 0 15px;
            margin-bottom: 30px;
        }
        .demailerror {
            font-size: 12px;
            line-height: 1em;
            margin-top: 5px;
            color: #ff4848;
        }
        .dshowrequired {
            display: inline-block;
            background: #e7c6c6;
            line-height: 1em;
            font-size: 12px;
            /* border: 1px solid red; */
            color: red;
            margin-bottom: 5px;
            padding: 8px 15px;
            border-radius: 10px;
            margin-top: 5px;
        }

        .drefvalue {
            display: block;
            width: 100%;
            background: #fff;
            color: #7a7b7d;
            padding: 15px 18px;
            line-height: 1em;
            border-radius: 7px;
            border: 1px solid #d8d8d8;
            margin-top: 10px;
        }

        .dshowapplicationerror .dmaintitle {
            font-size: 14px;
            font-weight: bold;
            color: #ff3b3b;
            margin-top: 20px;
        }

        .dshowapplicationerror .dsubmain {
            font-size: 12px;
            font-weight: bold;
            color: #ff3b3b;
        }
        
        @media only screen and (max-width: 1060px) {
            .dforminfo .row > .col {
                width: 100%;
                flex-basis: inherit;
            }

            .dforms .dpositionitem {
                width: 100%;
            } 
        }
    </style>