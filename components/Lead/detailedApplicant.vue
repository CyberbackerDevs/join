<template>
    <div class="dselectedinner">
        <!-- <pre>{{info}}</pre> -->
        <div class="dheadselectedinfo" :style="{ background: 'url(' + require('@/assets/images/placeheader.jpg') + ') center right no-repeat' }">
            <h2>{{info.account.name}}</h2>
            <div class="additioanl_details">{{info.account.email}} | {{info.mobile}} | {{info.birthdate}}</div>
            <div class="daddress">{{info.address}}</div>
            <div class="dstats">
                <div class="dsitems">
                    Date Applied: <span>{{info.assessment.updated_at}}</span>
                </div>
                <div class="dsitems">
                    <!-- Consultant: <span>{{info.consultant.cvalue}}</span> -->
                </div>
                <div class="dsitems">
                    Status: <span>{{info.status}}</span>
                </div>
                <div class="dsitems">
                    Referred By: <span v-if="info.referral !== null">{{info.referral}}</span><span v-if="info.referral === null">none</span>
                </div>
                <div class="dsitems">
                    Origin: <span>{{info.hearfrom}}</span>
                </div>
            </div>
            <div class="apply-pos">
                <h4>Position that they are applying for*</h4>
                <div class="ap-item" v-for="posti in info.positions" :key="posti">
                    {{ posti }},
                </div>
            </div>
            <div class="diserops">
                <div class="dinfoitem"><a :href="info.resume" target="_blank">View Resume</a></div>
                <div class="dinfoitem" v-if="info.status == 'CBPool'"><a href="#" v-on:click="openBioModal">View Bio</a></div>
                <div class="dinfoitem" v-if="info.assessment !== null"><a :href="'/showchart?uuid='+info.account.api_token" target="_blank">View chart for {{info.account.name}}</a></div>
                <div class="dinfoitem" v-if="info.assessment === null"><a :href="'/assessment/step2?uuid='+info.account.api_token" target="_blank">Continue Application for {{info.account.name}}</a></div>
            </div>
            <v-dialog v-model="openUserProfile" persistent max-width="890" content-class="d-user-profile-modal">
                <div class="d-modal-user-profiles">
                    <div class="d-modal-user-profiles-info">
                        <div class="d-modal-user-left">
                            <div class="d-main-left-container" :style="{ background: 'url(' + require('@/assets/images/landscape.jpg') + ') center 80% no-repeat' }">
                                <div class="d-user-profile-image">
                                    <img src="~/assets/images/sample_photo.jpeg" alt="">
                                </div>
                                <div class="d-user-profile-name">Cyberbacker {{info.firstname}}</div>
                                <div class="d-user-profile-applying-for">
                                    <span class="ap-item" v-for="posti in info.positions" :key="posti">
                                        {{ posti }},
                                    </span>
                                </div>
                                <div class="d-icon-cluster">
                                    <div class="d-icon-base">
                                        <a href="#" target="_blank" title="Introduction Video"><font-awesome-icon icon="play" /></a>
                                        <a href="#" target="_blank" title="Applicant Resume"><font-awesome-icon icon="file" /></a>
                                        <a href="#" target="_blank" title="Background Checking"><font-awesome-icon icon="search" /></a>
                                        <a href="#" target="_blank" title="Internet Test Result"><font-awesome-icon icon="wifi" /></a>
                                        <a href="#" target="_blank" title="Workstation Specification"><font-awesome-icon icon="desktop" /></a>
                                    </div>
                                </div>
                                <!-- <div class="d-user-profile-bio">Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. </div> -->
                            </div>
                        </div>
                        <div class="d-modal-user-right">
                            <div class="d-modal-user-informations">
                                <div class="d-modal-close-now">
                                    <font-awesome-icon icon="times" v-on:click="closeUserProfileModal" />
                                </div>
                                <div class="d-modal-user-information-item">
                                    <div class="d-modal-personal-info">
                                        <div class="d-item-full-spread d-personal-name">Hi! im, {{info.firstname}}</div>
                                    </div>
                                </div>
                                <div class="d-modal-user-information-item form-items-list">
                                    <h2 class="d-moda-recent-work-experiene-title">Recent Experience</h2>
                                    <div class="d-modal-recent-work-experience">
                                        <div class="d-modal-form-item">
                                            <label for="">Starting Date</label>
                                            <div class="d-modal-date-format">
                                                <div class="d-date-format-month">
                                                    <select v-model="bio.starting_month" name="" id="">
                                                        <option value="">Select a Month</option>
                                                        <option :value="mon" v-for="mon in months" :key="mon">{{mon}}</option>
                                                    </select>
                                                </div>
                                                <div class="d-date-format-year">
                                                    <input v-model="bio.starting_year" type="text" name="" id="" placeholder="XXXX">
                                                </div>
                                            </div>
                                        </div>
                                        <div class="d-modal-form-item">
                                            <label for="">Ending Date</label>
                                            <div class="d-modal-date-format">
                                                <div class="d-date-format-month">
                                                    <select v-model="bio.ending_month" name="" id="">
                                                        <option value="">Select a Month</option>
                                                        <option :value="mon" v-for="mon in months" :key="mon">{{mon}}</option>
                                                    </select>
                                                </div>
                                                <div class="d-date-format-year">
                                                    <input v-model="bio.ending_year" type="text" name="" id="" placeholder="XXXX">
                                                </div>
                                            </div>
                                        </div>
                                        <div class="d-modal-form-item">
                                            <label for="">Project Position</label>
                                            <input v-model="bio.position" type="text" name="" id="">
                                        </div>
                                        <div class="d-modal-form-item">
                                            <label for="">Project Description</label>
                                            <textarea v-model="bio.responsibility" name="" id="" cols="30" rows="10"></textarea>
                                        </div>
                                    </div>
                                    <h2 class="d-moda-recent-work-experiene-title">Other Info</h2>
                                    <div class="d-modal-recent-work-experience">
                                        <div class="d-modal-form-item">
                                            <label for="">Introduction Video Link</label>
                                            <input v-model="bio.introduction_video" type="text" name="" id="">
                                        </div>
                                        <div class="d-modal-form-item">
                                            <label for="">Primary Workstation Details</label>
                                            <input v-model="bio.workstation_main_details" type="text" name="" id="">
                                        </div>
                                        <div class="d-modal-form-item">
                                            <label for="">Secondary Workstation Details</label>
                                            <input v-model="bio.workstation_backup_details" type="text" name="" id="">
                                        </div>
                                        <!-- <div class="d-modal-form-item">
                                            <label for="">Internet Speed Test Result</label>
                                            <input v-model="bio.internet_speed_test" type="text" name="" id="">
                                        </div> -->
                                    </div>
                                </div>
                                <div class="d-modal-save-form">
                                    <button v-on:click="processbio">Save Bio</button>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </v-dialog>
        </div>
        <div class="reply_based" >
            <!-- <div class="forexpecationscall">User Updated for</div> -->
            <!-- <div class="info-item dconsultants" v-if="info.consultant.cvalue == 'none'">
                <div class="cbcunsolbutton" v-if="showConsultantButton">
                    <button @click="addCareerPeople()">Assign Career Consultant</button>
                </div>
                <div class="dassigconsultantfield"  v-if="!showConsultantButton">
                    <v-select
                        v-model="selectedRole"
                        :items="career"
                        item-text="name"
                        item-value="userid"
                        solo
                        class="dcareerlist"
                    ></v-select>
                    <button class="saveButton" @click="saveConsultant(info.account.id, info.account.api_token)">Save Consultant</button> <button class="cancelButton" @click="cancelAdd()">Cancel</button> 
                </div>
            </div> -->

            <!-- change details -->
            <div class="d-change-applicant-info">
                <v-dialog v-model="changeApplicantDetails" width="500" >
                    <template v-slot:activator="{ on, attrs }">
                        <v-btn color="lighten-2 update-profile-button" dark v-bind="attrs" v-on="on" >Update Applicant Info</v-btn>
                    </template>

                    <v-card>
                        <v-card-title class="text-h5 grey lighten-2 update-applicant-details">Update Applicant Details</v-card-title>

                        <v-card-text>
                            <div class="d-updating-modal">
                                <div class="d-input-item">
                                    <v-text-field v-model="toUpdateApplicantInfo.fname" label="First Name"></v-text-field>
                                </div> 
                                <div class="d-input-item">
                                    <v-text-field v-model="toUpdateApplicantInfo.lname" label="Last Name"></v-text-field>
                                </div> 
                                <div class="d-input-item">
                                    <v-text-field v-model="toUpdateApplicantInfo.email" label="Email"></v-text-field>
                                </div> 
                                <div class="d-input-item">
                                    <v-text-field v-model="toUpdateApplicantInfo.phone" label="Phone"></v-text-field>
                                </div>
                                <div class="d-input-item">
                                    <v-text-field v-model="toUpdateApplicantInfo.address" label="Address"></v-text-field>
                                </div>
                            </div> 
                        </v-card-text>

                        <v-divider></v-divider>

                        <v-card-actions>
                        <v-spacer></v-spacer>
                        <v-btn color="primary" text v-on:click="saveUpdateApplicantInfo">Save</v-btn>
                        </v-card-actions>
                    </v-card>
                </v-dialog>
            </div> 
            
            <div class="info-item">
                <!-- <h3>Y-assessment</h3> -->
                <!-- <div v-if="Object.keys(interviewDetails).length > 0"> -->
                <LeadInterviewDetails :info="this.info" />
                <div class="ya_result">
                    <!-- <div class="ya_bar_item" v-for="(itm, index) in info.assessment" :key="index" v-if="index == 'aesthetic' || index == 'altruist' || index == 'economic' || index == 'individualistic' || index == 'political' || index == 'regulatory' || index == 'theoretical'"  >
                        <div class="as-bar-vals">
                            <div class="dbar-vals" :style="{'height': (((parseInt(itm) / 100) * 100) * 2)+'px', 'background': color[index]}">
                                
                            </div>
                            <div class="dpassing" :style="{'bottom': (((parseInt(average[index]) / 100) * 100) * 2)+'px'}"></div>
                            <div class="dhigh" :style="{'bottom': (((parseInt(high[index]) / 100) * 100) * 2)+'px'}"></div>
                            <div class="dlow" :style="{'bottom': (((parseInt(low[index]) / 100) * 100) * 2)+'px'}"></div>
                            <div class="dnumber">{{itm}}</div>
                            <div class="dbar-text">
                                {{index}}
                            </div>
                        </div>
                    </div> -->
                    <div class="ya_bar_item">
                        <div class="as-bar-vals">
                            <div class="dbar-vals" :style="{'height': (((parseInt(info.assessment.aesthetic) / 100) * 100) * 2)+'px', 'background': color['aesthetic']}">
                                
                            </div>
                            <div class="dpassing" :style="{'bottom': (((parseInt(average['aesthetic']) / 100) * 100) * 2)+'px'}"></div>
                            <div class="dhigh" :style="{'bottom': (((parseInt(high['aesthetic']) / 100) * 100) * 2)+'px'}"></div>
                            <div class="dlow" :style="{'bottom': (((parseInt(low['aesthetic']) / 100) * 100) * 2)+'px'}"></div>
                            <div class="dnumber">{{info.assessment.aesthetic}}</div>
                            <div class="dbar-text">aesthetic</div>
                        </div>
                    </div>
                    <div class="ya_bar_item">
                        <div class="as-bar-vals">
                            <div class="dbar-vals" :style="{'height': (((parseInt(info.assessment.altruist) / 100) * 100) * 2)+'px', 'background': color['altruist']}">
                                
                            </div>
                            <div class="dpassing" :style="{'bottom': (((parseInt(average['altruist']) / 100) * 100) * 2)+'px'}"></div>
                            <div class="dhigh" :style="{'bottom': (((parseInt(high['altruist']) / 100) * 100) * 2)+'px'}"></div>
                            <div class="dlow" :style="{'bottom': (((parseInt(low['altruist']) / 100) * 100) * 2)+'px'}"></div>
                            <div class="dnumber">{{info.assessment.altruist}}</div>
                            <div class="dbar-text">altruist</div>
                        </div>
                    </div>
                    <div class="ya_bar_item">
                        <div class="as-bar-vals">
                            <div class="dbar-vals" :style="{'height': (((parseInt(info.assessment.economic) / 100) * 100) * 2)+'px', 'background': color['economic']}">
                                
                            </div>
                            <div class="dpassing" :style="{'bottom': (((parseInt(average['economic']) / 100) * 100) * 2)+'px'}"></div>
                            <div class="dhigh" :style="{'bottom': (((parseInt(high['economic']) / 100) * 100) * 2)+'px'}"></div>
                            <div class="dlow" :style="{'bottom': (((parseInt(low['economic']) / 100) * 100) * 2)+'px'}"></div>
                            <div class="dnumber">{{info.assessment.economic}}</div>
                            <div class="dbar-text">economic</div>
                        </div>
                    </div>
                    <div class="ya_bar_item">
                        <div class="as-bar-vals">
                            <div class="dbar-vals" :style="{'height': (((parseInt(info.assessment.individualistic) / 100) * 100) * 2)+'px', 'background': color['individualistic']}">
                                
                            </div>
                            <div class="dpassing" :style="{'bottom': (((parseInt(average['individualistic']) / 100) * 100) * 2)+'px'}"></div>
                            <div class="dhigh" :style="{'bottom': (((parseInt(high['individualistic']) / 100) * 100) * 2)+'px'}"></div>
                            <div class="dlow" :style="{'bottom': (((parseInt(low['individualistic']) / 100) * 100) * 2)+'px'}"></div>
                            <div class="dnumber">{{info.assessment.individualistic}}</div>
                            <div class="dbar-text">individualistic</div>
                        </div>
                    </div>
                    <div class="ya_bar_item">
                        <div class="as-bar-vals">
                            <div class="dbar-vals" :style="{'height': (((parseInt(info.assessment.political) / 100) * 100) * 2)+'px', 'background': color['political']}">
                                
                            </div>
                            <div class="dpassing" :style="{'bottom': (((parseInt(average['political']) / 100) * 100) * 2)+'px'}"></div>
                            <div class="dhigh" :style="{'bottom': (((parseInt(high['political']) / 100) * 100) * 2)+'px'}"></div>
                            <div class="dlow" :style="{'bottom': (((parseInt(low['political']) / 100) * 100) * 2)+'px'}"></div>
                            <div class="dnumber">{{info.assessment.political}}</div>
                            <div class="dbar-text">political</div>
                        </div>
                    </div>
                    <div class="ya_bar_item">
                        <div class="as-bar-vals">
                            <div class="dbar-vals" :style="{'height': (((parseInt(info.assessment.regulatory) / 100) * 100) * 2)+'px', 'background': color['regulatory']}">
                                
                            </div>
                            <div class="dpassing" :style="{'bottom': (((parseInt(average['regulatory']) / 100) * 100) * 2)+'px'}"></div>
                            <div class="dhigh" :style="{'bottom': (((parseInt(high['regulatory']) / 100) * 100) * 2)+'px'}"></div>
                            <div class="dlow" :style="{'bottom': (((parseInt(low['regulatory']) / 100) * 100) * 2)+'px'}"></div>
                            <div class="dnumber">{{info.assessment.regulatory}}</div>
                            <div class="dbar-text">regulatory</div>
                        </div>
                    </div>
                    <div class="ya_bar_item">
                        <div class="as-bar-vals">
                            <div class="dbar-vals" :style="{'height': (((parseInt(info.assessment.theoretical) / 100) * 100) * 2)+'px', 'background': color['theoretical']}">
                                
                            </div>
                            <div class="dpassing" :style="{'bottom': (((parseInt(average['theoretical']) / 100) * 100) * 2)+'px'}"></div>
                            <div class="dhigh" :style="{'bottom': (((parseInt(high['theoretical']) / 100) * 100) * 2)+'px'}"></div>
                            <div class="dlow" :style="{'bottom': (((parseInt(low['theoretical']) / 100) * 100) * 2)+'px'}"></div>
                            <div class="dnumber">{{info.assessment.theoretical}}</div>
                            <div class="dbar-text">theoretical</div>
                        </div>
                    </div>
                </div>
                <div class="ya_result_values">
                    <h2 class="valassest">Values Assessment</h2>
                    <div class="dvalspackage">
                        <div class="dsumitem dva-aesthetic">
                            <!-- BOF Aesthetic -->
                                <div class="summary-item" v-if="info.assessment.aesthetic <= 40">
                                    <h2>Low Aesthetic</h2>
                                    <div class="desc">You are not into artistic expression, or achieving balance and harmony in life. You are all about the utilitarian, bottom-line results.</div>
                                </div>
                                <div class="summary-item" v-if="info.assessment.aesthetic <= 51 && info.assessment.aesthetic > 40">
                                    <h2>Average Aesthetic</h2>
                                    <div class="desc">You are able to appreciate the benefit for balance and harmony without losing sight of the practical side of things.</div>
                                </div>
                                <div class="summary-item" v-if="info.assessment.aesthetic <= 65 && info.assessment.aesthetic > 51">
                                    <h2>High Aesthetic</h2>
                                    <div class="desc">You very much prefer form, harmony and balance. You are likely a strong advocate for green initiatives and protecting personal time and space.</div>
                                </div>
                                <div class="summary-item" v-if="info.assessment.aesthetic > 65">
                                    <h2>Very High Aesthetic</h2>
                                    <div class="desc">You place great importance in finding a good work-life balance, creating more than destroying and artistic self-expression.</div>
                                </div>
                            <!-- EOF Aesthetic -->
                        </div>
                        <div class="dsumitem dva-altruist">
                            <!-- BOF Altruist -->
                                <div class="summary-item" v-if="info.assessment.altruist <= 40">
                                    <h2>Low Altruist</h2>
                                    <div class="desc">You won't be taken advantage of and protect your own turf and that of the team or organization.</div>
                                </div>
                                <div class="summary-item" v-if="info.assessment.altruist <= 51 && info.assessment.altruist > 40">
                                    <h2>Average Altruist</h2>
                                    <div class="desc">You are concerned for others without giving everything away; a stabilizer.</div>
                                </div>
                                <div class="summary-item" v-if="info.assessment.altruist <= 70 && info.assessment.altruist > 51">
                                    <h2>High Altruist</h2>
                                    <div class="desc">You have a high desire to help others learn, grow, and develop.</div>
                                </div>
                                <div class="summary-item" v-if="info.assessment.altruist > 70">
                                    <h2>Very High Altruist</h2>
                                    <div class="desc">You have a very high sincerity-factor and a high empathy for others' needs.</div>
                                </div>
                            <!-- EOF Altruist -->
                        </div>
                        <div class="dsumitem dva-economic">
                            <!-- BOF Economic -->
                                <div class="summary-item" v-if="info.assessment.economic <= 44">
                                    <h2>Low Economic</h2>
                                    <div class="desc">You are a team player and may put others’ needs before yoursaved_paged_params.type</div>
                                </div>
                                <div class="summary-item" v-if="info.assessment.economic <= 58 && info.assessment.economic > 44">
                                    <h2>Average Economic</h2>
                                    <div class="desc">You are able to perceive and create a balance between the need for economic return and other needs as well.</div>
                                </div>
                                <div class="summary-item" v-if="info.assessment.economic <= 80 && info.assessment.economic > 58">
                                    <h2>High Economic</h2>
                                    <div class="desc">Your high drive for economic gain helps provide motivation through long projects and assignments.</div>
                                </div>
                                <div class="summary-item" v-if="info.assessment.economic > 80">
                                    <h2>Very High Economic</h2>
                                    <div class="desc">You are very competitive and bottom-line oriented.</div>
                                </div>
                            <!-- EOF Economic -->
                        </div>
                        <div class="dsumitem dva-individualistic">
                            <!-- BOF Individualistic -->
                                <div class="summary-item" v-if="info.assessment.individualistic <= 44">
                                    <h2>Low Individualistic</h2>
                                    <div class="desc">You are able to support the efforts of the team without demanding the limelight; a supportive team player.</div>
                                </div>
                                <div class="summary-item" v-if="info.assessment.individualistic <= 53 && info.assessment.individualistic > 44">
                                    <h2>Average Individualistic</h2>
                                    <div class="desc">You are not an extremist and able to balance the needs of both others and self.</div>
                                </div>
                                <div class="summary-item" v-if="info.assessment.individualistic <= 66 && info.assessment.individualistic > 53">
                                    <h2>High Individualistic</h2>
                                    <div class="desc">You have no problem standing up for your own rights and may impart this energy into others as well.</div>
                                </div>
                                <div class="summary-item" v-if="info.assessment.individualistic > 66">
                                    <h2>Very High Individualistic</h2>
                                    <div class="desc">You demonstrate high independence and project self-confidence.</div>
                                </div>
                            <!-- EOF Individualistic -->
                        </div>
                        <div class="dsumitem dva-political">
                            <!-- BOF Political -->
                                <div class="summary-item" v-if="info.assessment.political <= 41">
                                    <h2>Low Political</h2>
                                    <div class="desc">You are supportive of the efforts of the team; no hidden agendas. Willing to surrender control.</div>
                                </div>
                                <div class="summary-item" v-if="info.assessment.political <= 52 && info.assessment.political > 41">
                                    <h2>Average Political</h2>
                                    <div class="desc">You are flexible, able to take or leave the power or clout that comes with the job title or assignment.</div>
                                </div>
                                <div class="summary-item" v-if="info.assessment.political <= 65 && info.assessment.political > 52">
                                    <h2>High Political</h2>
                                    <div class="desc">You are able to accept the credit or take the blame with a 'the buck stops here' attitude.</div>
                                </div>
                                <div class="summary-item" v-if="info.assessment.political > 65">
                                    <h2>Very High Political</h2>
                                    <div class="desc">You are a very strong leader, and able to take control of a variety of initiatives and maintain control.</div>
                                </div>
                            <!-- EOF Political -->
                        </div>
                        <div class="dsumitem dva-regulatory">
                            <!-- BOF Regulatory -->
                                <div class="summary-item" v-if="info.assessment.regulatory <= 32">
                                    <h2>Low Regulatory</h2>
                                    <div class="desc">You are able to be a multi-threaded problem solver, able to shift gears and projects in a flexible way.</div>
                                </div>
                                <div class="summary-item" v-if="info.assessment.regulatory <= 41 && info.assessment.regulatory > 32">
                                    <h2>Average Regulatory</h2>
                                    <div class="desc">You are able to balance and understand the need to have structure and order, but not paralyzed without it.</div>
                                </div>
                                <div class="summary-item" v-if="info.assessment.regulatory <= 56 && info.assessment.regulatory > 41">
                                    <h2>High Regulatory</h2>
                                    <div class="desc">You have a strong preference for following established systems or creating them if none present.</div>
                                </div>
                                <div class="summary-item" v-if="info.assessment.regulatory > 56">
                                    <h2>Very High Regulatory</h2>
                                    <div class="desc">You are well disciplined, and follow standard operating protocol and traditional ways.</div>
                                </div>
                            <!-- EOF Regulatory -->
                        </div>
                        <div class="dsumitem dva-theoretical">
                            <!-- BOF theoretical -->
                                <div class="summary-item" v-if="info.assessment.theoretical <= 31">
                                    <h2>Low Theoretical</h2>
                                    <div class="desc">You are less concerned with the big picture or knowledge for knowledge sake.</div>
                                </div>
                                <div class="summary-item" v-if="info.assessment.theoretical <= 52 && info.assessment.theoretical > 32">
                                    <h2>Average Theoretical</h2>
                                    <div class="desc">You are able to balance the quest for understanding and knowledge with the practical needs of a situation.</div>
                                </div>
                                <div class="summary-item" v-if="info.assessment.theoretical <= 70 && info.assessment.theoretical > 52">
                                    <h2>High Theoretical</h2>
                                    <div class="desc">You have a high interest level in understanding all aspects of a situation or subject.</div>
                                </div>
                                <div class="summary-item" v-if="info.assessment.theoretical > 70">
                                    <h2>Very High Theoretical</h2>
                                    <div class="desc">You are passionate about learning for its own sake. You are continually in learning mode and bringing a very high degree of technical or knowledge base credibility.</div>
                                </div>
                            <!-- EOF theoretical -->
                        </div>
                    </div>
                </div>
            </div>

            <div v-if="info.status == 'CBPool'" class="assign_to_packets">
                <div class="assign_to_packets_inner">
                    <h3>Packets Involved</h3>
                    <div v-if="loadingPackets" class="d-packet-loading-part">loading......</div>
                    <div v-if="!loadingPackets"  class="d-packet-list">
                        <div class="d-packet-no-item" v-if="packetsInfo.length <= 0">No assigned packet yet</div>
                        <div class="d-packet-item" v-for="(packet, key) in packetsInfo" :key="key">
                            <div class="d-packet-inner">
                                <h3>{{packet.client_name}}</h3>
                                <div class="d-packet-desc">{{packet.client_desc}}</div>
                                <div class="d-packet-options">
                                    <div class="d-packet-option-left">
                                        <div class="d-packet-cb-number">{{packet.cyberbacker.length}}</div>
                                        <div class="d-packet-cb-label">In pool talents</div>
                                    </div>
                                    <div class="d-packet-option-right">
                                        <v-progress-circular v-if="removeItemLoading" indeterminate color="white"></v-progress-circular>
                                        <button v-if="!removeItemLoading" v-on:click='removePacketItem(packet.id, key)'>Remove</button>
                                    </div>
                                </div>
                            </div>
                        </div>
                        
                    </div>
                    <div class="d-packet-item full-width" v-on:click="toogleOpenAddToPacket">
                            <div class="d-packet-inner add packet">
                                <div class="d-add-icon">+ Add user to Packet</div>
                            </div>
                        </div>
                    <v-dialog v-model="openAddToPacket" persistent max-width="490" content-class="d-user-profile-modal">
                        <div class="d-modal-add-to-packet">
                            <div class="d-add-packet-item">
                                <label for="">Choose from the packets</label>
                                <div class="d-packet-list">
                                    
                                    <div v-if="allActivePackets.length <= 0" class="d-packet-list-item-loading">Loading...</div>
                                    <div v-if="allActivePackets.length > 0 && !savingPacket" class="d-packet-list-item" disabled>
                                        <div v-for="packet in allActivePackets" :key="packet.id" class="d-packet-list-one">
                                            <div class="d-packet-title">{{packet.client_name}}</div>
                                            <div class="d-packet-desc">{{packet.client_desc}}</div>
                                            <div class="d-packet-button" v-on:click="addUserToPacket(packet.id)">+</div>
                                        </div>
                                    </div>
                                    <div v-if="savingPacket" class="d-packet-saving">saving.....</div>
                                </div>
                                
                                <div class="d-packet-modal-option"> 
                                    <button v-on:click="closeAddPacket">Close</button>
                                </div>
                            </div>
                        </div>
                    </v-dialog>
                </div>
            </div>

            <div class="info-item">
                <h3>Senses*</h3>
                <div class="sense_list">
                    <div class="dsenselist" v-for="posti in info.assessment.senses" :key="posti">
                        &#9745; {{ posti }}
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import { library } from '@fortawesome/fontawesome-svg-core'
    import { faCheckCircle, faTimesCircle} from '@fortawesome/free-regular-svg-icons'
    import { faArrowRight, faArrowLeft } from '@fortawesome/free-solid-svg-icons'
    import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'

    library.add(faCheckCircle, faTimesCircle, faArrowRight, faArrowLeft)

export default {
    name: 'detailedApplicant',
    props: [
        'info'
    ],
    components: {
        FontAwesomeIcon
    },
    data(){
        return {
            changeApplicantDetails: false,
            showConsultantButton: true,
            openUserProfile: false,
            openAddToPacket: false,
            savingPacket: false,
            removeItemLoading: false,
            loadingPackets: true,
            y_assessment: [],
            position: [],
            packetsInfo: [],
            allActivePackets: [],
            consultant: '',
            selectedRole: '',
            career: [],
            bio:{
                starting_month: '',
                starting_year: '',
                ending_month: '',
                ending_year: '',
                position: '',
                responsibility: '',
                introduction_video: '',
                workstation_main_details: '',
                workstation_backup_details: '',
                internet_speed_test: '',
            },
            toUpdateApplicantInfo: {
                fname: '',
                lname: '',
                email: '',
                phone: '',
                address: ''
            },
            assessmentCounter: 0,
            color: {
                'aesthetic': '#3eb8ed',
                'altruist': '#ffc5c3',
                'economic': '#c0e0db',
                'individualistic': '#eed472',
                'political': '#97cdef',
                'regulatory': '#feb9e3',
                'theoretical': '#fac8ac',
            },
            average: {
                'aesthetic': 41,
                'altruist': 40,
                'economic': 46,
                'individualistic': 46,
                'political': 43,
                'regulatory': 35,
                'theoretical': 43,
            },
            high: {
                'aesthetic': 51,
                'altruist': 53,
                'economic': 58,
                'individualistic': 57,
                'political': 53,
                'regulatory': 42,
                'theoretical': 55,
            },
            low: {
                'aesthetic': 31,
                'altruist': 27,
                'economic': 34,
                'individualistic': 36,
                'political': 33,
                'regulatory': 28,
                'theoretical': 31,
            },
            months: ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December']

        }
    },
    methods: {
        addCareerPeople(){
            this.showConsultantButton = false;
        },
        cancelAdd(){
            this.showConsultantButton = true;
        },
        getConsultant(){
            let self = this;
            const headers = {
                'Access-Control-Allow-Origin': '*'
            }

            const params = {
                "role": ["Career Backer"]
            }

            this.$axios.get('https://be2.applytocyberbacker.com/api/cb/byRole', {params}, { headers: headers })
            .then(function (response) {
                console.log('show careers -> ', response.data.data.info);
                self.career = response.data.data.info;
                // window.location.reload()
            })
            .catch(function (error) {
                console.log('show error ->', error);
            });
        },
        saveConsultant(id, token){
            console.log('consultant ->', this.consultant);

            const headers = {
                'Access-Control-Allow-Origin': '*'
            }

            const params = {
                "userid": id,
                "type": "career_consultant",
                "cvalue": this.selectedRole
            }

            this.$axios.post('https://be2.applytocyberbacker.com/api/users/insert/consultant', params, { headers: headers })
            .then(function (response) {
                console.log('info -> ', response);
                window.location.reload()
            })
            .catch(function (error) {
                console.log('show error ->', error);
            });
        },
        openBioModal(){
            console.log('show bio now');
            this.openUserProfile = true;
        },
        closeUserProfileModal(){
            console.log('close modal');
            this.openUserProfile = false;
        },
        getUserBio(){
            // console.log('get the user bio -> ', this.info);

            let config = {
                headers: {'Access-Control-Allow-Origin': '*'},
                params: {
                    user_id: this.info.id
                },
            }
            
            let self = this;
            this.$axios.get('https://be2.applytocyberbacker.com/api/packet/users/profile', config)
            .then(function (response) {
                console.log('show user bio -> ', response.data.data);
                console.log('show user account -> ', response.data.data.info.account);

                let dbioresponse = response.data.data;

                self.bio.position = dbioresponse.recent_exp_position;
                self.bio.responsibility = dbioresponse.recent_exp_responsibility;
                self.bio.introduction_video = dbioresponse.introduction_video;
                self.bio.workstation_main_details = dbioresponse.workstation_main_details;
                self.bio.workstation_backup_details = dbioresponse.workstation_backup_details;
                self.bio.internet_speed_test = dbioresponse.internet_speed_test;

                /**
                 * split starting date
                 */
                if(dbioresponse.recent_exp_datefrom != ''){
                    let startinginfo = dbioresponse.recent_exp_datefrom.split(' ');
                    self.bio.starting_month = startinginfo[0];
                    self.bio.starting_year = startinginfo[1];
                }

                if(dbioresponse.recent_exp_dateto != ''){
                    let endinginfo = dbioresponse.recent_exp_dateto.split(' ');
                    self.bio.ending_month = endinginfo[0];
                    self.bio.ending_year = endinginfo[1];
                }
                

                
                // self.career = response.data.data.info;
                // window.location.reload()
            })
            .catch(function (error) {
                console.log('show error ->', error);
            });
        },
        processbio(){
            // console.log('bio info =->', this.bio);
            console.log('user info =->', this.info.id);
            const headers = {
                'Access-Control-Allow-Origin': '*'
            }

            let bioinfo  = {
                user_id: this.info.id,
                data: {
                    recent_exp_datefrom: this.bio.starting_month +" "+ this.bio.starting_year,
                    recent_exp_dateto: this.bio.ending_month +" "+ this.bio.ending_year,
                    recent_exp_position: this.bio.position,
                    recent_exp_responsibility: this.bio.responsibility,
                    introduction_video: this.bio.introduction_video,
                    workstation_main_details: this.bio.workstation_main_details,
                    workstation_backup_details: this.bio.workstation_backup_details,
                    internet_speed_test: this.bio.internet_speed_test
                }
            }

            this.$axios.post('https://be2.applytocyberbacker.com/api/packet/users/profile', bioinfo, { headers: headers })
            .then(function (response) {
                console.log('info -> ', response);
                window.location.reload()
            })
            .catch(function (error) {
                console.log('show error ->', error);
            });



            console.log(bioinfo);

        },
        getUserPacket(){
            let config = {
                headers: {'Access-Control-Allow-Origin': '*'},
                params: {
                    userid: this.info.id
                },
            }
            
            let self = this;
            this.$axios.get('https://be2.applytocyberbacker.com/api/packet/user/packets', config)
            .then(function (response) {
                console.log('show user packets -> ', response.data.data);
                self.packetsInfo = response.data.data;
                self.loadingPackets = false;
            })
            .catch(function (error) {
                console.log('show error ->', error);
            });
        },
        closeAddPacket(){
            this.openAddToPacket = false;
        },
        toogleOpenAddToPacket(){
            console.log('add to packet');
            this.openAddToPacket = true;
            this.getActivePackets();
        },
        getActivePackets(){
            let config = {
                headers: {'Access-Control-Allow-Origin': '*'},
                params: {
                    userid: this.info.id,
                },
            }
            
            let self = this;
            this.$axios.get('https://be2.applytocyberbacker.com/api/packet/active', config)
            .then(function (response) {
                console.log('show active packets -> ', response.data.data.packet);
                self.allActivePackets = response.data.data.packet;
            })
            .catch(function (error) {
                console.log('show error ->', error);
            });
        },
        addUserToPacket(packetID){
            console.log('selected packet -> ', packetID);
            this.savingPacket = true;

            const headers = {
                'Access-Control-Allow-Origin': '*'
            }

            let bioinfo  = {
                userid: this.info.id,
                packet: packetID
            }

            let self = this;
            this.$axios.post('https://be2.applytocyberbacker.com/api/packet/user/assign', bioinfo, { headers: headers })
            .then(function (response) {
                console.log('info -> ', response);
                self.getUserPacket();
                self.closeAddPacket();
                self.savingPacket = false;
                // window.location.reload()
            })
            .catch(function (error) {
                console.log('show error ->', error);
            });
        },
        removePacketItem(packetID, key){
            // console.log('dkey -> ', key);
            this.removeItemLoading = true;
            // console.log('remove from lost');

            const headers = {
                'Access-Control-Allow-Origin': '*'
            }

            let bioinfo  = {
                userid: this.info.id,
                packet: packetID
            }

            let self = this;
            this.$axios.post('https://be2.applytocyberbacker.com/api/packet/user/unassign', bioinfo, { headers: headers })
            .then(function (response) {
                console.log('info -> ', response);
                self.getUserPacket();
                self.closeAddPacket();
                self.removeItemLoading = false;
                // window.location.reload()
            })
            .catch(function (error) {
                console.log('show error ->', error);
            });
        },
        saveUpdateApplicantInfo(){
            console.log('update me -> ', this.toUpdateApplicantInfo);

            // update the applicant info
            const headers = {
                'Access-Control-Allow-Origin': '*'
            }

            let bioinfo  = {
                applicant_id: this.info.account.id,
                fname: this.toUpdateApplicantInfo.fname,
                lname: this.toUpdateApplicantInfo.lname,
                email: this.toUpdateApplicantInfo.email,
                phone: this.toUpdateApplicantInfo.phone,
                address: this.toUpdateApplicantInfo.address
            }

            console.log('this -> ', bioinfo);

            let self = this;
            this.$axios.post('https://be2.applytocyberbacker.com/api/users/update/info', bioinfo, { headers: headers })
            .then(function (response) {
                console.log('info -> ', response);
                // self.getUserPacket();
                // self.closeAddPacket();
                // self.savingPacket = false;
                window.location.reload()
            })
            .catch(function (error) {
                console.log('show error ->', error);
            });

        },
        assignDefaults(){
            this.toUpdateApplicantInfo.fname = this.info.firstname;
            this.toUpdateApplicantInfo.lname = this.info.lastname;
            this.toUpdateApplicantInfo.email = this.info.account.email;
            this.toUpdateApplicantInfo.phone = this.info.mobile;
            this.toUpdateApplicantInfo.address = this.info.address;

        }
    },
    mounted(){
        this.getConsultant();
        this.getUserBio();
        this.getUserPacket();
        this.assignDefaults();
    }
}
</script>

<style scoped>
    .dheadselectedinfo {
        background-size: cover !important;
        padding: 30px 30px 15px;
        color: #fff;
        position: relative;
    }
    .dheadselectedinfo h2 {
        font-size: 26px;
        font-weight: 400 !important;
        line-height: 1em;
        margin-bottom: 5px;
    }
    .dselectedinner {
        border-radius: 5px;
        overflow: hidden;
    }
    .dstats div.dsitems {
        display: inline-block;
        margin-right: 25px;
        vertical-align: middle;
        line-height: 1em;
        font-size: 12px;
        font-weight: 200;
    }

    .dstats div.dsitems span {
        display: block;
        font-weight: 400;
        margin-top: 5px;
    }

    .additioanl_details {
        margin-bottom: 7px;
        line-height: 1em;
        font-size: 14px;
        font-weight: 100;
    }

    .daddress {
        margin-bottom: 10px;
    }

    .sendinvite {
        text-align: right;
    }

    .reply_based {
        background: #fff;
        padding: 30px 30px 15px;
        /* font-size: 14px;
        font-style: italic; */
    }

    .reply_based span {
        font-weight: bold;
        display: block;
    }

    .ya_item {
        display: inline-block;
        width: 140px;
        margin-bottom: 20px;
    }

    .ya_bar_item {
        display: inline-block;
        width: 11%;
        margin: 0px 1%;
    }

    .ya_bar_item .as-bar-vals {
        position: relative;
    }

    .ya_bar_item .as-bar-vals .dnumber {
        font-size: 14px;
        font-weight: bold;
        line-height: 1em;
        margin-top: 5px;
        margin-bottom: 2px;
    }

    .ya_bar_item .as-bar-vals .dbar-text {
        line-height: 1em;
        text-transform: capitalize;
        padding: 0px;
        font-size: 12px;
        font-weight: bold;
    }

    .ya_bar_item .dpassing {
        border: 1px solid #e3ff0a70;
        position: absolute;
        width: 100%;
    }
    .ya_bar_item .dhigh {
        border: 1px solid #0bff0a70;
        position: absolute;
        width: 100%;
    }
    .ya_bar_item .dlow {
        border: 1px solid #f4666670;
        position: absolute;
        width: 100%;
    }

    .ya_bar_item .dcheckmarks .dacticon.green-base {
        font-size: 20px;
        margin-bottom: 10px;
        color: green;
    }

    .ya_bar_item .dcheckmarks .dacticon.failed-base {
        font-size: 20px;
        margin-bottom: 10px;
        color: red;
    }

    .ya_bar_item .dbar-vals {
        width: 55px;
        margin: 0 auto;
        border-radius: 5px;
    }

    .ya_result_values {
        margin-top: 20px;
    }

    .ya_result_values h2.valassest {
        text-transform: uppercase;
        font-weight: 100;
        background: #4c5f99;
        color: #fff;
        text-align: center;
        line-height: 1em;
        padding: 7px;
        letter-spacing: 1px;
        margin-bottom: 15px;
    }

    .ya_result_values .dvalspackage .dsumitem {
        display: inline-block;
        width: 47%;
        margin: 0 1% 10px;
        vertical-align: top;
        border-radius: 5px;
        overflow: hidden;
    }

    .ya_result_values .dvalspackage .dsumitem h2 {
        font-weight: 400;
        text-transform: uppercase;
        font-size: 17px;
        line-height: 1em;
        padding: 7px 15px;
    }

    .ya_result_values .dvalspackage .dsumitem .desc {
        padding: 10px 15px;
        line-height: 1.3em;
        font-size: 12px;
    }

    .ya_result_values .dvalspackage .dsumitem.dva-aesthetic {
        background: #3eb8ed70;
    }
    .ya_result_values .dvalspackage .dsumitem.dva-economic {
        background: #c0e0db70;
    }
    .ya_result_values .dvalspackage .dsumitem.dva-political {
        background: #97cdef70;
    }
    .ya_result_values .dvalspackage .dsumitem.dva-altruist {
        background: #ffc5c370;
    }
    .ya_result_values .dvalspackage .dsumitem.dva-regulatory {
        background: #feb9e370;
    }
    .ya_result_values .dvalspackage .dsumitem.dva-theoretical {
        background: #fac8ac70;
    }
    .ya_result_values .dvalspackage .dsumitem.dva-individualistic {
        background: #eed47270;
    }

    .ya_result_values .dvalspackage .dsumitem.dva-aesthetic  .summary-item h2 {
        background: #3eb8ed;
    }
    .ya_result_values .dvalspackage .dsumitem.dva-economic  .summary-item h2 {
        background: #c0e0db;
    }
    .ya_result_values .dvalspackage .dsumitem.dva-political  .summary-item h2 {
        background: #97cdef;
    }
    .ya_result_values .dvalspackage .dsumitem.dva-altruist  .summary-item h2 {
        background: #ffc5c3;
    }
    .ya_result_values .dvalspackage .dsumitem.dva-regulatory  .summary-item h2 {
        background: #feb9e3;
    }
    .ya_result_values .dvalspackage .dsumitem.dva-theoretical .summary-item h2 {
        background: #fac8ac;
    }
    .ya_result_values .dvalspackage .dsumitem.dva-individualistic .summary-item h2 {
        background: #eed472;
    }

    .score {
        text-align: center;
        background-size: cover!important;
        padding: 8px;
        margin: 0 auto;
        width: 89px;
    }

    .snum {
        background: rgb(138,82,248);
        background: linear-gradient(137deg, rgba(138,82,248,1) 0%, rgba(224,92,216,1) 100%);
        font-size: 25px;
        font-weight: 100;
        line-height: 1em;
        height: 73px;
        width: 73px;
        margin: 0 auto;
        border-radius: 90px;
        color: #fff;
        padding-top: 34%;
    }

    .dlabel {
        font-size: 14px;
        margin-top: 9px;
        line-height: 1em;
    }

    .ya_result {
        text-align: center;
    }
    

    .info-item > h3 {
        font-size: 16px;
        font-weight: 400;
        margin-bottom: 15px;
    }

    .social_list {
        margin-bottom: 25px;
    }

    .social_list > div {
        display: inline-block;
        margin-right: 10px;
    }

    .positions_list {
        margin-bottom: 25px;
    }

    .positions_list > div {
        font-size: 13px;
        line-height: 1em;
        border: 1px solid #4d62a3;
        display: inline-block;
        padding: 10px 15px;
        color: #4d62a3;
        margin-right: 10px;
        border-radius: 15px;
        margin-bottom: 10px;
    }

    .sense_list {
        margin-bottom: 20px;
    }
    .sense_list .dsenselist {
        margin-bottom: 10px;
    }
    .dstats div.dsitems.sendinvite {
        position: absolute;
        top: 15px;
        right: 0;
    }
    .dconsultants {
        margin-bottom: 25px;
    }
    .dassigconsultantfield input[type="text"] {
        width: 64%;
        line-height: 1em;
        border: 1px solid #c2c2c2;
        padding: 10px 20px;
        border-radius: 10px;
        box-shadow: 0 0 12px #c2c2c2;
    }
    .dassigconsultantfield .dcareerlist {
        width: 64%;
        display: inline-block;
    } 
    
    .dassigconsultantfield button.saveButton {
        background: #fe608b;
        background: linear-gradient(137deg, #fe608b 0%, #ff99a2 100%);
        color: #fff;
        line-height: 1em;
        padding: 15px 20px;
        border-radius: 6px;
    }
    .dassigconsultantfield button.cancelButton {
        margin-left: 10px;
        font-weight: bold;
    }
    .cbcunsolbutton button {
        background: #fe608b;
        background: linear-gradient(137deg, #fe608b 0%, #ff99a2 100%);
        color: #fff;
        line-height: 1em;
        padding: 15px 30px;
        border-radius: 50px;
    }
    .apply-pos {
        margin-top: 15px;
    }
    .apply-pos h4 {
        font-weight: 100;
        display: block;
    }
    .apply-pos .ap-item {
        display: inline-block;
        margin-right: 5px;
        font-size: 12px;
    }
    .diserops {
        margin-top: 15px;
    }
    .diserops .dinfoitem {
        display: inline-block;
        margin-right: 10px;
    }
    .diserops .dinfoitem a {
        background: #fe608b;
        background: linear-gradient(137deg, #fe608b 0%, #ff99a2 100%);
        line-height: 1em;
        text-decoration: none;
        color: #fff;
        font-size: 12px;
        padding: 9px 20px;
        display: block;
        border-radius: 30px;
    }
    .info-item {
        margin-bottom: 15px;
    }

    /* bio modal */
    .d-modal-user-profiles {
        background: #fff;
        font-family: 'Poppins' !important;
    }
    .d-main-left-container {
        display: inline-block;
        width: 100%;
        padding: 100px 0px;
        min-height: 650px;
    }
    .d-modal-user-left {
        display: inline-block;
        width: 50%;
        vertical-align: middle;
    }
    .d-modal-user-right {
        display: inline-block;
        width: 49%;
        vertical-align: middle;
    }
    .d-user-profile-image {
        text-align: center;
        margin: 0px 0 40px;
    }
    .d-user-profile-image img {
        width: 180px;
        border-radius: 200px;
        box-shadow: 0 0 16px #363638;
        border: 2px solid #fff;
    }
    .d-user-profile-name {
        text-align: center;
        font-size: 30px;
        font-weight: 300;
        color: #fff;
        margin-bottom: 15px;
    }
    .d-user-profile-bio {
        padding: 0 50px;
        font-size: 16px;
        font-weight: 300;
        color: #fff;
        text-align: center;
    }
    .d-item-full-spread.d-personal-name {
        font-size: 24px;
        font-weight: 400;
        line-height: 1em;
    }
    .d-item-list-spread {
        display: inline-block;
        font-size: 15px;
        line-height: 1em;
    }
    .d-personal-marital-address {
        font-size: 15px;
        line-height: 1em;
    }
    .d-modal-personal-info {
        margin-bottom: 20px;
    }
    .d-moda-recent-work-experiene-title {
        font-weight: 400;
        font-size: 20px;
        margin-bottom: 10px;
    }
    .d-recent-year-ranged {
        font-size: 15px;
        font-style: italic;
        font-weight: 400;
        line-height: 1em;
    }
    .d-recent-company {
        font-weight: 600;
        font-size: 16px;
        line-height: 1.2em;
        margin-bottom: 15px;
    }
    .d-recent-year-reponsibility {
        max-height: 200px;
        overflow-y: scroll;
        font-size: 15px;
        line-height: 1.3em;
    }
    .d-modal-user-informations {
        padding: 15px 35px;
        position: relative;
    }
    .d-recent-year-reponsibility ul li {
        list-style: none;
        margin-bottom: 15px;
    }
    .d-modal-recent-work-experience {
        margin-bottom: 20px;
    }
    .d-other-list-item {
        display: inline-block;
        margin-bottom: 5px;
        padding: 7px 10px;
        background: #363638;
        line-height: 1em;
        border-radius: 5px;
    }
    .d-other-list-item a {
        text-decoration: none;
        font-size: 13px;
        line-height: 1em;
        color: #fff;
    }
    .d-user-profile-applying-for {
        text-align: center;
        color: #fff;
        font-size: 14px;
        font-weight: 300;
    }
    .d-icon-cluster {
        text-align: center;
        margin-top: 45px;
    }
    .d-icon-base a {
        display: inline-block;
        border: 2px solid #fff;
        color: #fff;
        line-height: 1em;
        height: 45px;
        width: 45px;
        padding-top: 12px;
        text-align: center;
        border-radius: 50px;
        margin-right: 10px;
    }
    .d-icon-base a:hover {
        background: #fff;
        color: #363638;
    }
    .d-modal-close-now {
        position: absolute;
        top: -5px;
        right: 10px;
        z-index: 9;
        cursor: pointer;
    }

    .d-modal-recent-work-experience .d-modal-form-item {
        margin-bottom: 10px;
    }

    .d-modal-recent-work-experience .d-modal-form-item label{
        display: block;
        font-size: 14px;
        margin-bottom: 5px;
    }

    .d-modal-recent-work-experience .d-modal-form-item input{
        display: block;
        border: 1px solid #ccc;
        width: 100%;
        line-height: 1em;
        border-radius: 5px;
        padding: 5px 10px;
        font-size: 14px;
    }
    .d-modal-recent-work-experience .d-modal-form-item textarea{
        display: block;
        height: 80px;
        border: 1px solid #ccc;
        width: 100%;
        border-radius: 5px;
        padding: 5px 10px;
        font-size: 14px;
    }
fname    .d-modal-date-format .d-date-format-month select {
        display: block;
        border: 1px solid #ccc;
        width: 100%;
        line-height: 1.2em;
        border-radius: 5px;
        padding: 7px 10px;
        font-size: 14px;
    }
    .form-items-list {
        max-height: 470px;
        overflow-y: scroll;
        margin-bottom: 20px;
    }
    .d-modal-save-form {
        text-align: right;
    }
    .d-modal-save-form button {
        display: inline-block;
        line-height: 1em;
        background: rgb(68, 68, 68);
        color: #fff;
        font-size: 14px;
        text-transform: uppercase;
        font-weight: bold;
        padding: 10px 15px;
    }
    .d-modal-date-format .d-date-format-month {
        display: inline-block;
        width: 60%;
    }
    .d-modal-date-format .d-date-format-year {
        display: inline-block;
        width: 38%;
    }

    .d-packet-item.full-width {
        width: 100%;
        border-radius: 60px;
        box-shadow: 0 0 12px #ff939f;
    }

    .d-packet-item.full-width .d-packet-inner {
        cursor: pointer;
        font-weight: bold;
        font-size: 13px;
        padding: 15px 30px;
        background: #fe608b;
    background: linear-gradient(137deg, #fe608b 0%, #ff99a2 100%);
    }

    .d-packet-item {
        width: 48%;
        margin-right: 1%;
        display: inline-block;
        margin-bottom: 1%;
        border-radius: 5px;
        overflow: hidden;
        box-shadow: 0 0 12px #6e7eb0;
    }
    .d-packet-inner {
        background: #4d619f;
        color: #fff;
        padding: 20px;
    }
    .d-packet-inner h3 {    
        line-height: 1em;
        margin-bottom: 10px;
        font-size: 16px;
        font-weight: 400;
    }

    .d-packet-inner .d-packet-desc {
        font-size: 12px;
        line-height: 1.2em;
        margin-bottom: 10px;
    }

    .d-packet-options .d-packet-option-left {
        display: inline-block;
        width: 48%;
        vertical-align: middle;
    }

    .d-packet-options .d-packet-option-right {
        display: inline-block;
        width: 48%;
        vertical-align: middle;
        text-align: right;
    }
    .d-packet-options .d-packet-option-left .d-packet-cb-number {
        font-weight: 300;
        font-size: 22px;
        line-height: 1em;
    }
    .d-packet-options .d-packet-option-left .d-packet-cb-label {
        font-size: 12px;
        line-height: 1em;
        font-style: italic;
    }
    .d-packet-options .d-packet-option-right button {
        background: #fe608b;
        background: linear-gradient(137deg, #fe608b 0%, #ff99a2 100%);
        line-height: 1em;
        text-decoration: none;
        color: #fff;
        font-size: 12px;
        padding: 9px 20px;
        border-radius: 30px;
    }

    .assign_to_packets_inner > h3 {
        margin-bottom: 15px;
        font-weight: 400;
        font-size: 18px;
    }

    .assign_to_packets {
        margin-bottom: 40px;
    }

    .d-modal-add-to-packet {
        background: #fff;
        padding: 20px 30px;
        border-radius: 5px;
    }

    .d-packet-list-item {
        max-height: 500px;
        overflow-y: scroll;
    }

    .d-packet-list-one {
        position: relative;
        border-bottom: 1px solid #ccc;
        padding: 0 10px 10px;
        margin-bottom: 10px;
    }

    .d-packet-list-one .d-packet-title {
        font-size: 18px;
        font-weight: 400;
        line-height: 1.2em;
        margin-bottom: 5px;
    }
    .d-packet-list-one .d-packet-desc {
        font-size: 13px;
        line-height: 1.2em;
        max-width: 75%;
    }
    .d-packet-list-one .d-packet-button {
        position: absolute;
        top: 10px;
        right: 0;
        background: #4d619f;
        color: #fff;
        line-height: 1em;
        padding: 7px 10px;
        border-radius: 50px;
        font-weight: 300;
        cursor: pointer;
    }

    .d-packet-modal-option button {
        background: #4d619f;
        color: #fff;
        line-height: 1em;
        padding: 10px 20px;
        border-radius: 50px;
        font-weight: 300;
    }

    .d-add-packet-item label {
        font-size: 18px;
        line-height: 1em;
        margin-bottom: 20px;
        display: block;
    }

    .d-packet-no-item {
        padding: 20px;
    }

    .update-applicant-details {
        font-size: 18px !important; 
    }

    .d-change-applicant-info button.v-btn.update-profile-button {
        background: #fe608b;
        background: linear-gradient(137deg,#fe608b,#ff99a2);
        width: 100%;
        border-radius: 5px;
        margin-bottom: 10px;
    }
</style>

<style>
    .v-list .v-list-item {
        min-height: initial;
    }
    .v-list .v-list-item:hover {
        background: #ececec;
    }
    .v-menu__content .v-list-item__content {
        padding: 5px 0;
    }
    .v-menu__content .v-list-item__content .v-list-item__title {
        font-size: 12px;
    }
    .d-user-profile-modal {
        border-radius: 0 !important;
    }
</style>