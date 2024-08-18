<template>
  <div class="main-container">
    <div class="content-container">
      <div class="profile-section">
        <div class="profile-card">
          <a :href="linkedIn" target="_blank" class="profile-link">
            <q-img src="../assets/IMG_9593 (1).jpg" class="profile-pic" />
          </a>
          <div class="profile-info">
            <p class="creator-info">Created by:</p>
            <p class="creator-name">{{ creatorName }}</p>
            <p class="visit-link">Connect with me on <a :href="linkedIn" target="_blank">LinkedIn</a></p>
          </div>
        </div>
      </div>
      <div class="survey-section">
        <h2 class="survey-title">NYC Job Interest Survey</h2>
        <h3 class="survey-subtitle">Your Future Awaits!</h3>
        <div class="survey-container">
          <p class="survey-description">We're excited to hear your career interests! This anonymous survey will help us understand your preferences and guide you towards a fulfilling career. <span class="highlighted-text">Your feedback is valuable.</span></p>

          <q-select
            outlined
            v-model="selectedSchool"
            :options="displayList"
            label="Choose your high school"
            option-value="SchoolID"
            option-label="SchoolName"
            use-input
            input-debounce="0"
            @filter="filterSchool"
            clearable
            class="school-select"
          >
            <template v-slot:option="scope">
              <q-item v-bind="scope.itemProps" class="school-option">
                <q-item-section>
                  <q-item-label lines="1">
                    {{ scope.opt.SchoolName }}
                  </q-item-label>
                </q-item-section>
              </q-item>
            </template>
            <template v-slot:no-option>
              <q-item>
                <q-item-section class="no-results">No options found</q-item-section>
              </q-item>
            </template>
          </q-select>

          <div class="input-group">
            <q-input outlined v-model="email" label="Your Email Address" class="email-input" @focus="error = false" @keyup.enter="go"/>
            <q-btn color="primary" class="submit-btn" @click="go">Submit</q-btn>
          </div>
          <p v-if="schoolError" class="error-message">Please select a school.</p>
          <p v-if="emailError" class="error-message">Invalid email address.</p>
        </div>
      </div>
    </div>
  </div>
</template>


<script>
export default {
 data() {
   return {
     email: '',
     emailError: false,
     schoolError: false,
     loading: false,
     selectedSchool: null,
     schoolList: [],
     displayList: [],

   }
 },
 watch: {
   selectedSchool() {
     if(this.selectedSchool !== null){
       this.schoolError = false
     }
   },
   email() {
     if(this.email.length > 0){
       this.emailError = false
     }
   }
 },
 computed: {
   creatorName () {
     return process.env.CREATOR_NAME
   },
   linkedIn () {
     return process.env.LINKEDIN
   },
   linkedIn_pic () {
     return process.env.LINKEDIN_PIC
   }
 },
 methods: {
   filterSchool(val, update){
         if (val === '') {
             update(() => {
                 this.displayList = this.schoolList
             })
             return
         }
         update(() => {
             const needle = val.toLowerCase()
             this.displayList = this.schoolList.filter(rec => rec.SchoolName.toLowerCase().indexOf(needle) > -1)
         })
     },
   go() {
     if (/^\w+([\.-]?\w+)*@\w+([\.-]?\w+)*(\.\w{2,3})+$/.test(this.email)) {
       if(this.selectedSchool !== null ){
         this.$router.push({ name: 'Survey', query: { user: this.email, id: this.selectedSchool.SchoolID } })
       }else{
         this.schoolError = true
       }
     } else {
       this.emailError = true
     }
   }
 },
 mounted() {
     // load school list for select drop down box
     fetch(process.env.SERVER_URL + '/api/school-list')
     .then(response => response.json())
     .then(schoolList => {
         this.schoolList = schoolList
         this.displayList = this.schoolList
     })
 }
}
</script>


<style>
/* Global Styles */
body {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  margin: 0;
  padding: 0;
  background-image: url("../assets/image copy.png");
  background-size: 100%;
  color: #333;
}

/* Main Container */
.main-container {
  padding: 20px;
}

/* Header Image */
.header-image {
  width: 100%;
  max-height: 400px;
  background: url('~assets/header-bg.jpg') no-repeat center center;
  background-size: cover;
  border-radius: 12px;
  margin-bottom: 20px;
}

/* Content Container */
.content-container {
  display: flex;
  flex-direction: column;
  align-items: center;
}

/* Profile Section */
.profile-section {
  display: flex;
  justify-content: center;
  margin-bottom: 30px;
}

.profile-card {
  background: #ffffff;
  border-radius: 15px;
  padding: 20px;
  box-shadow: 0 8px 16px rgba(0,0,0,0.2);
  text-align: center;
  max-width: 320px;
}

.profile-link {
  display: inline-block;
  border-radius: 50%;
  overflow: hidden;
}

.profile-pic {
  border-radius: 50%;
  width: 120px;
  height: 120px;
  border: 5px solid #00acc1;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.profile-link:hover .profile-pic {
  transform: scale(1.05);
  box-shadow: 0 12px 24px rgba(0,0,0,0.3);
}

.profile-info {
  margin-top: 15px;
}

.creator-info {
  font-size: 1.4em;
  color: #555;
  margin: 0;
}

.creator-name {
  font-weight: bold;
  color: #00acc1;
  font-size: 1.6em;
}

.visit-link a {
  color: #00796b;
  text-decoration: none;
  font-weight: bold;
}

.visit-link a:hover {
  text-decoration: underline;
}

/* Survey Section */
.survey-section {
  background: #ffffff;
  border-radius: 15px;
  padding: 20px;
  box-shadow: 0 8px 16px rgba(0,0,0,0.2);
  max-width: 800px;
  width: 100%;
}

.survey-title {
  color: #00acc1;
  font-size: 2.2em;
  margin-bottom: 10px;
}

.survey-subtitle {
  color: #00796b;
  font-size: 1.6em;
  margin-bottom: 20px;
}

.survey-description {
  color: #333;
  font-size: 1.2em;
  margin-bottom: 20px;
}

.highlighted-text {
  font-weight: bold;
  color: #00acc1;
}

/* Input and Button Styles */
.school-select {
  margin-bottom: 20px;
}

.school-option {
  background-color: #f1f8e9;
}

.no-results {
  color: #b0bec5;
}

.input-group {
  display: flex;
  align-items: center;
}

.email-input {
  flex: 1;
}

.submit-btn {
  margin-left: 10px;
  background: linear-gradient(45deg, #00acc1, #00796b);
  border: none;
  color: white;
  padding: 10px 20px;
  text-transform: uppercase;
  font-weight: bold;
}

.submit-btn:hover {
  background: linear-gradient(45deg, #00796b, #00acc1);
}

.error-message {
  color: #d32f2f;
  font-weight: bold;
  margin-top: 10px;
}
</style>


