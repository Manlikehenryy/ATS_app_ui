<script setup>
import { RouterLink } from 'vue-router'
import Navbar from '../components/Navbar.vue';
</script>

<template>
     <Navbar/>
  <div style="margin-top: -100px;" class="bg-white">
    
      <div v-if="showSpinner" class="container"> <div class="spinner loading loading-spinner"></div></div>
      <div v-if="showNoJob" class="container"> <h3 >No jobs available!</h3></div>
  <div v-for="job in jobs" :key="job.id" style="margin-bottom: -220px;"  class="relative isolate px-6  lg:px-8">
  <RouterLink :to="{name: 'viewJob', params:{id:job.id}}">
  
    <div class="mx-auto max-w-2xl py-32 sm:py-48 lg:py-56">
      <div class="text-center">
        <h1 class="text-4xl font-bold tracking-tight text-gray-900 sm:text-6xl">{{job.title}}</h1>
        <p class="mt-6 text-lg leading-8 text-gray-600">{{job.job_description.substring(0,200) }}</p>
        
        <div class="mt-10 flex items-center justify-center gap-x-6">
          <div style="background-color: rgb(74, 118, 180);" class="rounded-md  px-3.5 py-2.5 text-sm font-semibold text-white shadow-sm  focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 ">View Job</div>
          <div class="text-sm font-semibold leading-6 text-gray-900">Learn more <span aria-hidden="true">→</span></div>
        </div>
     
      </div>
    </div>
 
 </RouterLink>
 
  </div>




</div>

</template>

<style>
.container {
  position: relative;
  width: 100vw; /* Adjust as needed */
  height: 100vh; /* Adjust as needed */
}

.container h3 {
  font-weight: bolder;
  position: absolute;
  top: 40%;
  left: 50%;
  transform: translate(-50%, -50%);
  max-width: 100%; /* Ensure the image doesn't exceed the container's width */
  max-height: 100%; /* Ensure the image doesn't exceed the container's height */
}
.container .spinner {
  width: 3rem;
  position: absolute;
  top: 30%;
  left: 50%;
  transform: translate(-50%, -50%);
  max-width: 100%; /* Ensure the image doesn't exceed the container's width */
  max-height: 100%; /* Ensure the image doesn't exceed the container's height */
}
</style>

<script>
   
   import axios from "axios";
   
    export default {
    
     data(){
     return {
      jobs:[],
      showSpinner: true,
      showNoJob: false
     }
     },
     mounted(){
      this.getAllJobs()
     },
     methods:{
       async getAllJobs(){
     
       
      await axios.get('https://ats-backend-pov9.onrender.com/api/job').then((res)=>{

       if (res.data.status=="success") {
        this.showSpinner = false;
        this.jobs = res.data.data
        
        if (res.data.data.length == 0) {
          this.showNoJob = true;
        }
        
       }

      }).catch((err)=>{  
        alert('something went wrong')
        console.log(err)}); 
       },
       viewJob(){
        this.$router.push({name: 'applicant', params:{id:2}})
       }

     }
   }
   </script>