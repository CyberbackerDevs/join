<template>
    <div class="notif_main">
        <div class="notif-inner">
            <h2>Attendance Overall</h2>
            <template>
              <v-data-table
                dense
                :headers="headers"
                :items="allAttendance"
                class="elevation-1"
              >
               <template v-slot:items="props">
                  <td>{{ props.items.attendancedate }}</td>
                   <td>{{ new Date(props.items.clockin).toLocaleString() }}</td>
                   <td>{{ new Date(props.items.clockout).toLocaleString() }}</td>
                 </template>
              </v-data-table>
            </template>
     </div>
    </div>
    
</template>

<script>
import moment from "moment"
export default {
    name: 'notif',
    props: [
        'title',
        'subtitle'
    ],
    methods: {
        changeDateFormat(date){
            const day = moment(date).format('MMMM Do YYYY hh:mm:ss');
            return day;

        },
        showAllAttendance(){
            let token_info = localStorage.getItem('token_info');
            let userId = localStorage.getItem('userId');
            let self = this;
            let header = {
                "Access-Control-Allow-Origin": "*",
                "Access-Control-Allow-Methods": "GET, POST, PATCH, PUT, DELETE, OPTIONS",
                "Access-Control-Allow-Headers": "Origin, Content-Type, X-Auth-Token"
            }
            let params = {
                userid: userId
            }
            self.showLoading = true;
            this.$axios.get('https://be2.applytocyberbacker.com/api/awesomenesstracker/user/attendances', {params}, { headers: header })
            .then(function (response) {
                self.loading = false
                let attendances = response.data.data.attendance;
                let lastElem = attendances.slice(-1);
                let currentSignIn = lastElem[0]["clockin"];
                let currentSignOut = lastElem[0]["clockin"];
                let attendanceDate = lastElem[0]["attendancedate"];
                self.lastLogin = lastElem[0]["clockin"];
                self.lastLogout = lastElem[0]["clockout"];
                const day = moment(attendanceDate).format('MMMM Do YYYY');
                self.lastAttendance = day;
                self.allAttendance = attendances;
            })
            .catch(function (error) {
                console.log(error);
            });

        },
        showCurrentAttendance(){
            let token_info = localStorage.getItem('token_info');
            let userId = localStorage.getItem('userId');
            console.log(userId)
            const current = new Date();
            const now = moment(current).format('YYYYMMDD');
            const time = moment(current).format('hh:mm a');
            console.log(time);
            console.log(now);
            let self = this;
            let header = {
                "Access-Control-Allow-Origin": "*",
                "Access-Control-Allow-Methods": "GET, POST, PATCH, PUT, DELETE, OPTIONS",
                "Access-Control-Allow-Headers": "Origin, Content-Type, X-Auth-Token"
            }
            let params = {
                attendancedate: now,
                userid: userId
            }
            self.showLoading = true;
            this.$axios.get('https://be2.applytocyberbacker.com/api/awesomenesstracker/attendance/today', {params}, { headers: header })
            .then(function (response) {
                self.loading = false
                console.log('segment info -> ', response.data.data);
                self.clockIn = response.data.data.attendance.clockin;
                self.clockOut = response.data.data.attendance.clockout;
                self.showLoading = false;
            })
            .catch(function (error) {
                console.log(error);
            });

        },
        clockAttendance(){
            let token_info = localStorage.getItem('token_info');
            let userId = localStorage.getItem('userId');
            console.log(userId)
            const current = new Date();
            const now = moment(current).format('YYYYMMDD');
            const time = moment(current).format('hh:mm a');
            console.log(now);
            let self = this;
            let header = {
                "Access-Control-Allow-Origin": "*",
                "Access-Control-Allow-Methods": "GET, POST, PATCH, PUT, DELETE, OPTIONS",
                "Access-Control-Allow-Headers": "Origin, Content-Type, X-Auth-Token"
            }
            let params = {
                attendancedate: now,
                userid: userId,
                clockattendance: time
            }
            this.showLoading = true;
            this.$axios.post('https://be2.applytocyberbacker.com/api/awesomenesstracker/saveattendance', params, { headers: header })
            .then(function (response) {
                window.location.reload();
            })
            .catch(function (error) {
                console.log(error);
            });
            
        },
        currentDate() {
          const current = new Date();
          const dateString = moment(current).format('MMMM Do, YYYY.');
          console.log(dateString) // Output: 2020-07-21
          return dateString
        },
        currentDateTime() {
              const current = new Date();
              const date =
                current.getFullYear() +
                "-" +
                (current.getMonth() + 1) +
                "-" +
                current.getDate();
              const time =
                current.getHours() +
                ":" +
                current.getMinutes() +
                ":" +
                current.getSeconds();
              const dateTime = date + " " + time;
              this.timestamp = dateTime;
        },
    },
    data(){
        return {
            clockIn: "",
            clockOut: "",
            clock: "In",
            showLoading: false,
            timestamp: '',
            lastLogin: '',
            lastLogout: '',
            lastAttendance: '',
            allAttendance: [],
            headers: [
                  {text: 'Attendance Date', value: 'attendancedate'},
                  {text: 'Clock In Date Time', value: 'clockin'},
                  {text: 'Clock Out Date Time', value: 'clockout'}
              ],
            
        }
    },
    mounted: function (){
        this.showCurrentAttendance();
        this.showAllAttendance();
        setInterval(() => { this.currentDateTime() }, 1000)
    }
}
</script>

<style scoped>
    .clock_in_button {
      background-color: #4CAF50; /* Green */
      border: none;
      color: white;
      padding: 15px 200px;
      text-align: center;
      text-decoration: none;
      display: inline-block;
      font-size: 16px;
    }
    .clock_out_button {
      background-color: #f5404c; /* Red */
      border: none;
      color: white;
      padding: 15px 200px;
      text-align: center;
      text-decoration: none;
      display: inline-block;
      font-size: 16px;
    }
    .notif_main {
        background: #fff;
        border-radius: 10px;
        margin-right: 20px;
        margin-top: 20px;
        box-shadow: 0px 0px 12px rgb(78 101 167 / 45%);
        position: relative;
    }
    .notif-inner {
        padding: 20px 30px;
        min-height: 160px;
    }
    .attendance_main {
        background: #fff;
        border-radius: 10px;
        margin-right: 20px;
        box-shadow: 0px 0px 12px rgb(78 101 167 / 45%);
        position: relative;
    }
    .attendance-inner {
        padding: 20px 30px;
        min-height: 160px;
    }
    .notif-inner h2 {
        margin-bottom: 15px;
        line-height: 1em;
        font-weight: 400;
    }
    .dmanimage {
        position: absolute;
        bottom: -1px;
        right: 30px;
    }
    .dmanimage img {
        width: 145px;
        vertical-align: bottom;
    }
    
    .dmessage {
        margin-left: 15px;
    }
    .dnotifitem {
        margin-bottom: 9px;
    }
    .dnotifitem span {
        background: #ff4747;
        color: #fff;
        height: 20px;
        width: 20px;
        text-align: center;
        font-size: 14px;
        display: inline-block;
        border-radius: 50px;
    }
</style>