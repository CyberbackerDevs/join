<template>
    <v-app>
        <div class="d-start-appliation" >
            <div class="d-start-inner" v-if="showSection == 'email'">
                <div class="d-start-header">
                    <h1>Hello, future <span>cyberbacker!</span></h1>
                </div>
                <p>Please be advised that our server is under maintenance. However, we always ensure to accommodate your applications. You can click on the link below to continue. We apologize for any inconvenience.</p>
                <div class="d-form-part">
                    <div class="d-form-item">
                        <div class="d-name-fields">
                            <div class="d-left-part">
                                <label for="">First Name</label>
                                <input type="text" v-model="bfname">
                            </div>
                            <div class="d-right-part">
                                <label for="">Last Name</label>
                                <input type="text" v-model="blname">
                            </div>
                        </div>
                        <label for="">Please provide your email address in the field below:</label>
                        <input type="text" name="d-user-email" v-model="emailChecking">
                        <div class="d-check-email-error" v-if="checkEmailErrorShow">Please enter a valid email address</div>
                        
                        <!-- <div v-if="bfname != '' && blname != '' && emailChecking != ''" class="d-application-combi-option">
                            <v-checkbox v-model="iHaveAgreeCombi">
                                <template v-slot:label>
                                    <div>
                                        I have read and agree to the terms of the
                                            <v-tooltip bottom>
                                                <template v-slot:activator="{ on }">
                                                    <button class="show-nda" v-on:click="openDocumentType">Non-Disclosure Agreement</button>
                                                </template>
                                                Click to show document
                                            </v-tooltip>
                                    </div>
                                </template>
                            </v-checkbox>
                        </div>
                         -->
                        <button @click="openDocumentType">Let's get started</button>

                        <!-- <button v-if="iHaveAgreeCombi" @click="checkEmailAvailability">Let's get started</button> -->
                        <v-dialog v-model="show_conditon" :width="condi_width">
                            <v-card>
                                <!-- <v-card-title class="text-h5 grey lighten-2">
                                Privacy Policy
                                </v-card-title> -->

                                <v-card-text>
                                    <div class="d-option-document">
                                        <div class="d-document-inner">
                                            <div class="d-sticky-header">
                                                <button v-on:click="closeDocu"><span>x</span> Close</button>
                                            </div>
                                            <div class="d-base-content">
                                                <div class="d-docu-one">
                                                    <h2>NON-DISCLOSURE AGREEMENT<br />PRIOR INITIAL INTERVIEW PROCESS</h2>
                                                    <h4>WARNING: No Recording and Copying </h4>
                                                    <p>The applicant shall not record, publish, or make a copy of the application process or any contract and document related hereto. The application process is the Cyberbacker, Inc.’s proprietary and intellectual property, therefore any reproduction, recording or reenactment of the application process is prohibited. In the event that the applicant records and copies this application process or any other document hereof, the applicant shall forfeit his/her application to Cyberbacker Inc. and shall be subjected to necessary legal action.</p>
                                                    <h4>No Affiliation clause</h4>
                                                    <p>I hereby certify that I am not currently associated/employed with any organization or business. I further certify that I am not currently working as a private or government employee, independent contractor, or in any other capacity. </p>
                                                    <h4>Authorization:</h4>
                                                    <p>I hereby declare that all information provided in this form is true and correct to the best of my knowledge. If thereafter that said information is found to be false, it shall be a sufficient ground to: (a) forfeit my application (if applicant); or (b) sever my relationship with Cyberbacker, Inc. (if independent contractor). Further, I shall solely be responsible for any and all disclosure I made in connection with my application with Cyberbacker, Inc. </p>
                                                    <p>I hereby authorize Cyberbacker Inc., its representatives, agents, and/or third-party providers to verify and confirm any and all information pertinent to my educational, employment, and personal background and history, including but not limited to the information that I provided in my application.</p>
                                                    <p>I affirm and consent to the disclosure and sharing of my personal information and sensitive personal information, to Cyberbacker, its agents, representatives, and/or third-party providers.</p>
                                                    <p>I hereby release, discharge, and waive any and all of my claims/cause of action against Cyberbacker, its representatives, agents, and/or third-party providers regarding my disclosed personal information. </p>
                                                    <p>I hereby declare that I am executing this declaration to attest to the truth of the foregoing statements. I was not coerced, forced, or intimidated in signing this declaration, as this was a product of my voluntary act and deed. </p>
                                                </div>
                                                <div class="d-docu-two">
                                                    <h2>NON-DISCLOSURE AGREEMENT<br />FOR ASPIRING CYBERBACKERS</h2>
                                                    <p class="centerme">This Non-Disclosure Agreement is made and entered between:</p>
                                                    <p class="centerme"><strong>Cyberbacker Inc.</strong>, a corporation duly organized and existing under the laws of the United States of America, with office address at 2447 Kiesel Ave Ogden, Utah 84401 represented by its CEO Craig Goodliffe, hereinafter referred to as <strong>“Cyberbacker Inc.”</strong></p>
                                                    <p class="centerme">and</p>
                                                    <p class="centerme"> <strong>{{bfname}} {{blname}}</strong>, of legal age, with sound mind and discretion, an applicant for Cyberbacker Inc. hereinafter referred to as <strong>“Applicant”</strong>.</p>
                                                    <p>The parties agree to enter into a confidential relationship with respect to the disclosure of certain intellectual and confidential information ("Confidential Information").</p>
                                                    <p><strong>1. Purpose.</strong> This Non-Disclosure Agreement (the <strong>“Agreement”</strong>) is created for the purpose of preventing the unauthorized disclosure of confidential and proprietary information of Cyberbacker Inc. to third parties, unauthorized persons, and its competitors.</p>
                                                    <p><strong>2. Definition of Confidential Information.</strong> "Confidential Information" shall include ALL information or material that has or could have commercial value or other utility in Cyberbacker Inc.’s business in which the latter is engaged, including but not limited to the application process, procedures, market niche, marketing strategies, intellectual property, know-how, methods, techniques, documents, trade secrets, assets, operational methods, contractual information, and the like, that are disclosed by reason of this Agreement or any other communication with Cyberbacker Inc. in trust and confidence and will not disclose any of such Confidential Information to any third party and competitors. </p>
                                                    <p><strong>3. Obligations of Applicant.</strong> The <strong>Applicant</strong> shall hold and maintain the Confidential  Information in the strictest confidence for the sole and exclusive benefit of Cyberbacker Inc. <strong>The Applicant shall not</strong>, without the prior written approval of Cyberbacker Inc., use for the <strong>Applicant’s</strong> own benefit, publish, copy, upload in internet platforms, or otherwise disclose to others, or permit the use by others for their benefit or to the detriment of Cyberbacker Inc., any Confidential Information. The <strong>Applicant</strong> shall return to Cyberbacker Inc. any and all records, notes, and other written, printed, or tangible materials in its possession pertaining to Confidential Information <strong>immediately</strong>. The Applicant shall <strong>immediately</strong>: (a) remove or destroy any and all tangible or intangible material acquired through any means; (b) give notice and submit proof to Cyberbacker Inc. of the actual destruction and removal of the same.</p>
                                                    <p><strong>4. Specific Performance; Indemnification; Notification.</strong> The <strong>Applicant</strong> acknowledges that Cyberbacker Inc. shall suffer irreparable damages resulting directly or indirectly in the event that the terms and conditions of this Agreement are violated. Further, Cyberbacker Inc. is entitled to damages, and shall seek an injunction to prevent <strong>Applicant</strong> from violating, or continuing to violate, this Agreement, notwithstanding, any other rights, remedies, or courses of action available under the law. <strong>The Applicant</strong> hereby undertakes to exempt Cyberbacker Inc. from any harm, loss, claim, liability, or expense attributable to or resulting from the <strong>Applicant's</strong> use or disclosure of Confidential Information without any authorization from the former. Furthermore, <strong>Applicant</strong> shall fully cooperate in any effort made by Cyberbacker Inc. to secure, protect, and redress such unauthorized use or disclosure. Finally, the <strong>Applicant</strong> hereby undertakes that he or she shall immediately notify Cyberbacker Inc. such <strong>Applicant</strong> becomes aware of any unauthorized disclosure or use of such Confidential Information.</p>
                                                    <p><strong>5. Time Periods.</strong> Unless otherwise released in writing, the terms and conditions of this Agreement shall remain effective and bind the Applicant until such Confidential Information, as determined by Cyberbacker, Inc., no longer qualifies as a trade secret or beneficial to Cyberbacker, Inc.</p>
                                                    <p><strong>6. Relationships.</strong> Nothing in this Agreement shall be construed to constitute the <strong>Applicant</strong> as a partner, joint venturer, independent contractor, or employee of Cyberbacker Inc.</p>
                                                    <p><strong>7. Severability.</strong> If any provision of this Agreement is hereafter declared invalid, illegal, or unenforceable under the law, all other provisions of this Agreement shall remain in effect.</p>
                                                    <p><strong>8. Governing Law.</strong> This Agreement is governed by, construed, applied, and enforced in accordance with laws of the State of Utah, without giving effect to its conflict of laws principles.</p>
                                                    <p><strong>9. Waiver.</strong> Nothing herein or hereafter shall be construed as a waiver by Cyberbacker, Inc. to enforce its rights under this Agreement and to undertake courses of action available under the law.</p>
                                                    <p><strong>10. Effective Date.</strong> This Non-Disclosure Agreement shall become effective and binding on the date and time the Applicant submitted "I agree" before taking the Y-Assessment.</p>
                                                    <p>I, the Applicant, acknowledge that I have read this Non-Disclosure Agreement in its entirety and hereby undertake to comply with its terms and conditions. I understand that I am required to abide by the provisions of this agreement in order to move forward with the Y-Assessment and other phases of this application process. Failure to agree will render my application to Cyberbacker Inc. void and forfeited.</p>
                                                </div>
                                                
                                            </div>
                                            <div class="d-proceed-options">
                                                    <v-checkbox class="d-checkbox-options" v-model="iHaveAgreeCombi">
                                                        <template v-slot:label>
                                                            <div>
                                                                I have read and agree to the terms of the Non-Disclosure Agreement
                                                            </div>
                                                        </template>
                                                    </v-checkbox>
													<div class="show_email_loading" v-if="showEmailLoading"><img :src="require('@/assets/images/loadingme.gif')" alt=""></div>
                                                    <button v-if="iHaveAgreeCombi" v-on:click="checkEmailAvailability">Continue Application</button>
													
                                                </div>
                                        </div>
                                    </div>
                                    
                                </v-card-text>

                                <!-- <v-card-actions>
                                <v-spacer></v-spacer>
                                <v-btn
                                    color="primary"
                                    text
                                    @click="dialog = false"
                                >
                                    I accept
                                </v-btn>
                                </v-card-actions> -->
                            </v-card>
                        </v-dialog>
                    </div>
                </div>
            </div>
            <div class="d-returning-email" v-if="showSection == 'returning'">
                <div class="d-start-header">
                    <h1>Welcome back<br /><span>{{emailCheckedInfo.name}}!</span></h1>
                </div>
                <div class="d-returning-message" v-if="returingMessageStatus == 'isAllowed'">
                    <div class="d-message-content">I see you are back to retake the application.<br />But since we already have your record,<br />you can proceed to the last step.</div>
                    <div class="d-returning-proceed-button">
                        <a :href="'/apply/assessment?uuid='+hasUserInfo.api_token">Proceed to y-assessment</a>
                    </div>
                </div>
                <div class="d-returning-message" v-if="returingMessageStatus == 'isNotAllowed'">
                    <div class="d-message-content">I see you are back to retake the application.<br />but unfortunately, you have already applied less than 60 days ago<br />Please come back on or after {{emailCheckedInfo.when_to_apply}} to<br />take your application again.</div>
                    <div class="d-returning-proceed-button">
                        <a href="https://cyberbacker.ph/">I understand, go back to home</a>
                    </div>
                </div>
            </div>
            <div class="d-new-application" v-if="showSection == 'uploadResume'">
                <div class="d-new-ask-email">
                    <div class="d-new-ask-email-header">Let's get <span>started!</span></div>
                    <div class="d-new-ask-email-desc">Please upload your resume in PDF format with a file size not exceeding 2MB.<br />Follow this naming format―Last Name-First Name-Middle Name.</div>
                    <div class="d-new-ask-email-form">
                        <input type="file" id="file" ref="file" v-on:change="selectedAFile()"/>
                    </div>
                    <div class="d-upload-resume-error" v-if="showUploadErrors">Please upload an appropriate resume, it must be less than 2MB and must be in pdf format</div>
                    <div class="d-new-ask-email-proceed" v-if="showProcessUploadResume">
                        <div class="d-loading-image" v-if="resumeUploading"><img :src="require('@/assets/images/loadingme.gif')" alt=""></div>
                        <div class="d-upload-resume" v-if="!resumeUploading">
                            <button @click="uploadNewResume">Upload Your Resume</button>
                        </div>
                    </div>
                </div>
            </div>
            <div class="d-new-application" v-if="showSection == 'addProfile'">
                <div class="d-new-ask-email">
                    <ApplyStepper step="1"/>
                    <div class="d-new-ask-email-desc">Please fill in the form. Do not leave anything blank.</div>
                    <div class="d-new-ask-email-form">
                        <div class="d-profile-item d-form-half" ref="firstnamepart">
                            <v-text-field ref="name" v-model="form.first_name" label="First name"></v-text-field>
                        </div>
                        <div class="d-profile-item d-form-half" ref="lastnamepart">
                            <v-text-field v-model="form.last_name" label="Last name"></v-text-field>
                        </div>
                        <div class="d-profile-item d-form-half">
                            <v-text-field label="Referral" v-model="form.referred_by"></v-text-field>
                        </div>
                        <div class="d-profile-item d-form-half" ref="emailpart">
                            <v-text-field label="Email address" v-model="form.email_address"></v-text-field>
                        </div>
                        <div class="d-profile-item d-form-half" ref="mobilepart">
                            <v-text-field v-model="form.mobile_number" label="Mobile number"></v-text-field>
                        </div>
                        <div class="d-profile-item d-form-half" ref="addresspart">
                            <v-text-field v-model="form.home_address" label="Address"></v-text-field>
                        </div>
                        <div class="d-profile-item d-form-third" ref="birthdatepart">
                            <v-select
                                v-model="birth_month"
                                :items="showMonths"
                                label="Birth Month" 
                            ></v-select>
                        </div>
                        <div class="d-profile-item d-form-third">
                            <v-select
                                v-model="birth_day" 
                                :items="showDates"
                                label="Birth Date"  
                            ></v-select>
                        </div>
                        <div class="d-profile-item d-form-third">
                            <v-select
                                v-model="birth_year" 
                                :items="showYear"
                                label="Birth Year"  
                            ></v-select>
                        </div>
                        <div class="d-profile-item d-form-full">
                            <v-textarea v-model="form.bio" label="Bio" ></v-textarea>
                        </div>
                        <div class="d-profile-item d-form-full" ref="socialpart">
                            <v-text-field v-model="form.facebook" label="Social Media Profiles" hint="Add active social media profile links" :rules="['Required']"></v-text-field>
                        </div>
                        <div class="d-profile-item d-form-full d-inline-checkbox"  ref="otherjobs">
                            <label for="">Other online jobs?</label>
                            <div class="dradiobots">
                                <v-radio-group v-model="form.otherjobs" row>
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
                        <div class="d-profile-item d-form-full d-inline-boxed">
                            <label for="">Where did you hear about us?</label>
                            <div class="dradiobots">
                                <v-radio-group v-model="form.hearfrom" row>
                                    <v-radio
                                        class="dpositionitem"
                                        v-for="(hears, index) in hearFromUs"
                                        v-bind:key="index"
                                        :label="hears"
                                        :value="hears"
                                    ></v-radio>
                                </v-radio-group>
                            </div>
                        </div>
                        <div class="d-profile-item d-form-full d-position-list"  ref="positionpart">
                            <label for="">The position/s you are applying for**</label>
                            <div class="d-positions-holder">
                                <div class="dpositionitem" v-for="(positon, index) in positions" v-bind:key="index">
                                    <v-checkbox
                                        v-model="form.positions"
                                        :label="positon.variation_value"
                                        :value="positon.variation_value"
                                    ></v-checkbox>
                                </div>
                            </div>
                        </div>
                        <div class="d-new-ask-email-proceed">
                            <div class="d-loading-image" v-if="proceedToStep2Loading"><img :src="require('@/assets/images/loadingme.gif')" alt=""></div>
                            <div class="d-upload-resume" v-if="!proceedToStep2Loading">
                                <button @click="proceedStepTwo">Proceed to Step 2</button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </v-app>
</template>

<script>
export default {
    head: {
        title: 'Application Start',
    },
    name: 'assessment_start',
    layout: 'Avtwo',
    data(){
        return {
			showEmailLoading: false,
            bfname: '', // used in initial UI
            blname: '', // used in initial UI
            show_conditon: false, // condition pop up
            iHaveAgreeCombi: false,
            condi_width: 900, // condition width
            proceedToStep2Loading: false,
            showSection: 'email', // default is email
            checkEmailErrorShow: false, 
            returingMessageStatus: 'isNotAllowed',
            isthird: false,
            resumeUploading: false,
            showProcessUploadResume: false,
            showUploadErrors: false,
            showforuploadcv: false,
            showDates: [],
            showMonths: ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'],
            showYear: [],
            // positions: [],
            hearFromUs: ["Facebook", "Instagram", "Twitter", "Onlinejobs.ph", "Craigslist", "Cyberbacker", "LinkedIn", "Youtube", "Tiktok", "Indeed", "Others", "Billboard", "Transition", "Jobstreet"],
			positions: ["Lead backer", "Transaction Backer", "Growth Backer", "Command Backer", "Cyberbacker", "Cyber Recruiter", "Graphic Artist", "Video Editor", "Client Relations", "Career Backer", "Legal Backer", "Listing Backer", "Agent Backer", "Concierge Backer", "Productivity Backer", "Money Backer", "Growth Parsed Appointment Backer", "Web Developer Backer", "Bookkeeper Backer", "Director of Industry", "Director of Client Rewards", "Marketing Backer", "SEO Backer", "Data Analyst Backer", "Graphic Backer", "Inbound\/Outbound Sales Agent", "Video Backer", "Social Media Backer", "Focus Group Manager", "Cryptocurrency Backer", "Data Admin Backer", "Logistics Admin Support Backer", "Website Management Support", "Executive Virtual Assistant", "Procurement and Contracts Processing Backer", "Executive Assistant Insurance Backer", "Appointment Setter Backer", "CyberCare Backer", "Screenwriter Backer","Email Correspondence Backer"],
            emailChecking: '',
            emailCheckedInfo: [],
            hasUserInfo: [],
            birth_day: '',
            birth_month: '',
            birth_year: '',
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
        }
    },
    methods: {
        uploadResume(){
            console.log('upload here', this.$refs.file.files[0]);
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
        getDate(){
            for (let index = 1; index < 32; index++) {
                this.showDates.push(index);
            }
        },
        getYear(){
            let baseYear = 1960;
            for (let index = baseYear; index < 2021; index++) {
                this.showYear.push(index);
            }
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
            console.log('this is the email ->', this.emailChecking);
			this.iHaveAgreeCombi = false;
			this.showEmailLoading = true;
			

            if(this.emailChecking == ''){
                this.checkEmailErrorShow = true;
                return;
            }
            

            this.form.email_address = this.emailChecking;
            this.form.first_name = this.bfname;
            this.form.last_name = this.blname;

            let self = this;
                
            const headers = {
                'Access-Control-Allow-Origin': '*',
            }

            let params = {
                "email": this.emailChecking
            }

            this.$axios.post('https://be2.applytocyberbacker.com/api/assessment/checkemail', params, { headers: headers })
            .then(function (response) {
                console.log('email -> ', response.data);
                if(Object.keys(response.data.data.email).length == 0){
                    self.showSection = 'uploadResume';
                } else {
                    self.emailCheckedInfo = response.data.data.email;
                    self.showSection = 'returning';
                    if(response.data.data.email.when_to_apply == 'now'){
                        self.returingMessageStatus = 'isAllowed';
                        self.hasUserInfo = response.data.data.email;
                    } else if(response.data.data.email.when_to_apply == '3rd'){
                        self.returingMessageStatus = 'isAllowed';
                        self.isthird = true;
                        self.hasUserInfo = response.data.data.email;
                    } else {
                        self.returingMessageStatus = 'isNotAllowed';
                    }
                }
                // self.positions = response.data.data.positions;
            })
            .catch(function (error) {
                console.log(error);
            });
            
        },
        openDocumentType(){
            this.show_conditon = true;
        },
        closeDocu(){
            this.show_conditon = false;
        },
        selectedAFile(){
            // console.log('selected resume ->', this.$refs.file.files[0]);
            this.showProcessUploadResume = false;
            this.showUploadErrors = false;
            let dUploadedResume = this.$refs.file.files[0];
            let hasError = false;
            if(dUploadedResume.type != 'application/pdf'){
                hasError = true;
                this.showUploadErrors = true;
            }

            if(dUploadedResume.size > 1999999){
                hasError = true;
                this.showUploadErrors = true;
            }

            if(hasError == false){
                this.showProcessUploadResume = true;
            }
        },
        uploadNewResume(){
            console.log('upload resume ->', this.$refs.file.files[0]);
            this.resumeUploading = true;

            let self = this;
                
            const headers = {
                'Access-Control-Allow-Origin': '*',
            }

            let formData = new FormData();
            formData.append('file', this.$refs.file.files[0]);

            this.$axios.post('https://be2.applytocyberbacker.com/api/resume/upload', formData, { headers: headers })
            .then(function (response) {
                // console.log('info -> ', response.data.data.resume);
                self.form.cv_link = "https://be2.applytocyberbacker.com/resume/"+response.data.data.resume;
                self.showSection = 'addProfile';
                // // console.log('length -> ', Object.keys(response.data.data.email).length);
                // if(Object.keys(response.data.data.email).length > 0){
                //     self.email_errors = "Email already registered"
                //     self.email_errors_show = true
                // }
            })
            .catch(function (error) {
                console.log(error);
                self.showUploadErrors = true;

            });

        },
        proceedStepTwo(){
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

            // build birthdate
            let bdate = this.birth_day;
            let bmonth = this.showMonths.indexOf(this.birth_month) + 1;

            if(bmonth <= 9){ bmonth = '0'+bmonth; }

            if(bdate <= 9){ bdate = '0'+bdate; }

            this.form.date_of_birth = this.birth_year+"-"+bmonth+"-"+bdate;


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
            // if(!this.reg.test(this.form.email_address)){
            //     this.email_errors_show = true;
            //     this.goto('emailpart');
            //     return;
            // }

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
            // if(this.form.cv_link == ""){
            //     this.showforuploadcv = true;
            //     this.goto('uploadcvresume');
            //     return;
            // }

            // check if bio is empty
            if(this.form.bio == ""){
                this.form.bio = 'none';
            }

            // refered by must be na if no referral
            if(this.form.referred_by === undefined){
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
                self.$router.push('/apply/senses?uuid='+response.data.data.api_token)
            })
            .catch(function (error) {
                console.log('some error on api');

                self.showProceedError = true;
                self.showLoading = false;


                console.log('error ->', error);
            });
        },
        getReferralName(){
            this.form.referred_by = (this.$route.query.ref == '' ? 'n/a' : this.$route.query.ref);
        }
    },
    mounted(){
        this.getDate();
        this.getYear();
        this.getPositions();
        this.getReferralName();
    }
}
</script>

<style scoped>
    .d-start-header h1 {
        font-family: 'Arimo', sans-serif;
        letter-spacing: 2px;
        font-size: 75px;
        line-height: 1em;
        text-shadow: 4px 4px 8px #ccc;
    }

    .d-start-header h1 span {
        color: #eb5600;
    }

    .d-new-ask-email-header {
        font-family: 'Arimo', sans-serif;
        letter-spacing: 2px;
        font-size: 91px;
        text-align: center;
        line-height: 1em;
        font-weight: bold;
        text-shadow: 4px 4px 8px #ccc;
    }

    .d-new-ask-email-header span {
        color: #eb5600;
    }

    .d-loading-image {
        text-align: center;
    }

    .d-loading-image img {
        width: 350px;
        margin-top: -50px;
    }

    .d-new-ask-email-form {
        text-align: center;
        margin-top: 40px;
    }

    .d-new-ask-email-form input {
        margin: 30px 0;
    }
    
    .d-new-ask-email-desc {
        text-align: center;
        margin: 15px 0;
    }
    .d-start-inner {
        font-family: 'Poppins';
        text-align: center;
        max-width: 950px;
        margin: 200px auto;
    }

    .d-returning-email {
        font-family: 'Poppins';
        text-align: center;
        max-width: 950px;
        margin: 200px auto;
    }

    .d-new-application {
        font-family: 'Poppins';
        /* text-align: center; */
        max-width: 950px;
        margin: 50px auto;
    }

    .d-start-header {
        margin-bottom: 40px;
    }

    .d-form-item {
        text-align: center;
    }
    .d-form-item label {
        display: block;
        margin-bottom: 10px;
    }

    .d-form-item input {
        display: block;
        margin: 0 auto;
        border: 2px solid #727272;
        width: 80%;
        line-height: 1em;
        border-radius: 10px;
        padding: 10px 20px;
        margin-bottom: 20px;
        box-shadow: 0 0 12px #c7c7c7;
    }

    .d-form-item button {
        display: block;
        margin: 0 auto;
        padding: 15px 60px;
        background: #f5f7fd;
        border-radius: 10px;
        transition: all 0.5s ease;
        box-shadow: 0 0 12px #c7c7c7;
    }

    .d-upload-resume button {
        display: block;
        margin: 0 auto;
        padding: 15px 60px;
        background: #f5f7fd;
        border-radius: 10px;
        transition: all 0.5s ease;
        box-shadow: 0 0 12px #c7c7c7;
    }

    .d-returning-proceed-button a{
        display: inline-block;
        margin: 30px auto;
        padding: 15px 60px;
        background: #f5f7fd;
        border-radius: 10px;
        transition: all 0.5s ease;
        box-shadow: 0 0 12px #c7c7c7;
        cursor: pointer;
        text-decoration: none;
    }

    .d-form-item button:hover {
        background: #505050;
        color: #fff;
        transition: all 0.5s ease;
    }

    .d-upload-resume button:hover {
        background: #505050;
        color: #fff;
        transition: all 0.5s ease;
    }

    .d-returning-proceed-button a:hover {
        background: #505050;
        color: #fff;
        transition: all 0.5s ease;
    }

    .d-message-content {
        font-size: 24px;
        line-height: 1.2em;
    }

    .d-new-ask-email-form input {
        background: #aeaeae;
        padding: 10px;
        border-radius: 5px;
    }

    .d-profile-item.d-form-half {
        display: inline-block;
        width: 45%;
        margin: 0 2%;
    }

    .d-profile-item.d-form-third {
        display: inline-block;
        width: 30%;
        margin: 0 1%;
    }

    .d-profile-item.d-form-full {
        margin: 0 3%;
    }

    .d-profile-item.d-form-full.d-inline-checkbox {
        text-align: left;
        margin-bottom: 20px;
    }

    .d-profile-item.d-form-full.d-inline-checkbox label {
        display: inline-block;
        vertical-align: middle;
        width: 20%;
    }   
    .d-profile-item.d-form-full.d-inline-checkbox .dradiobots {
        display: inline-block;
        vertical-align: middle;
    }

    .d-profile-item label {
        display: block;
        line-height: 1em;
        margin-bottom: 10px;
        text-align: left;
    }

    .d-profile-item input {
        margin: 0 0 25px;
        background: white;
        box-shadow: 0 0 12px #b3b3b3;
        width: 100%;
        display: inline-block;
    }

    .d-positions-holder {
        text-align: left;
    }

    .d-positions-holder .dpositionitem {
        width: 25%;
    }

    .dpositionitem {
        width: 22%;
        display: inline-block;
    }

    .d-profile-item.d-form-full.d-position-list label,
    .d-profile-item.d-form-full.d-inline-boxed label {
        margin-bottom: 20px;
    }

    .d-profile-item.d-form-full.d-inline-boxed,
    .d-profile-item.d-form-full.d-position-list {
        margin-bottom: 20px;
    }

    .d-check-email-error {
        color: #db0000;
        font-weight: bold;
        margin-bottom: 20px;
    }

    .d-upload-resume-error {
        text-align: center;
        color: #db0000;
        font-weight: bold;
        font-size: 14px;
    }

    .d-name-fields .d-left-part,
    .d-name-fields .d-right-part {
        display: inline-block;
        width: 49%;
    }

    .d-name-fields {
        width: 90%;
        margin: 0 auto;
    }

    .d-base-content {
        max-height: 600px;
        overflow-y: scroll;
    }

    .d-option-document {
        padding-top: 20px;
    }

    .d-document-inner h2 {
        text-align: center;
        margin-bottom: 30px;
        margin-top: 20px;
    }

    .d-docu-one P {
        padding-left: 30px;
        text-align: justify;
    }

    .d-docu-two p {
        text-align: justify;
    }
    .d-docu-two p.centerme {
        text-align: center;
    }

    .d-docu-one {
        box-shadow: 0 0 12px #a1a1a1;
        margin: 10px 10px 60px;
        border-radius: 3px;
        padding: 30px;
    }

    .d-docu-two {
        box-shadow: 0 0 12px #a1a1a1;
        margin: 10px 10px 10px;
        border-radius: 3px;
        padding: 30px;
    }

    .d-sticky-header {
        text-align: right;
        padding-bottom: 15px;
    }

    .d-sticky-header button {
        background: #747474;
        padding: 8px 14px 8px 9px;
        border-radius: 50px;
        color: #fff;
    }

    .d-sticky-header button span {
        background: #b7b7b7;
        color: #747474;
        line-height: 1em;
        padding: 2px 9px;
        border-radius: 50px;
        margin-right: 5px;
        font-weight: bold;
    }

    .d-options-show {
        padding-top: 20px;
    }

    .d-options-show button.proceed {
        display: block;
        margin: 0 auto;
        padding: 15px 60px;
        background: #f5f7fd;
        border-radius: 10px;
        transition: all 0.5s ease;
        box-shadow: 0 0 12px #eb5600;
    }

    .d-application-combi-option button.show-nda {
        background: none;
        margin: initial;
        display: inline-block;
        padding: 0;
        box-shadow: none;
        color: #eb5600;
    }

    .d-application-combi-option {
        text-align: center;
        width: 63%;
        margin: 0 auto;
    }

    .d-proceed-options {
        text-align: center;
        margin: 50px 0;
    }

    .d-proceed-options button {
        background: #007f2e;
        color: #fff;
        line-height: 1em;
        padding: 15px 25px;
        border-radius: 100px;
        box-shadow: 0 0 12px #009404;
        font-weight: bold;
    }

    .d-checkbox-options {
        margin: 0 auto !important;
        width: 73%;
    }

	.show_email_loading {
		height: 50px;
	}
	.show_email_loading img {
		width: 300px;
		margin-top: -85px;
	}
</style>

<style>
    body, html {
        background: #fff !important;
    }
    .theme--light.v-application {
        background: none !important;
    }

    .v-input--selection-controls {
        margin: 0 !important;
        padding: 0 !important;
    }
</style>