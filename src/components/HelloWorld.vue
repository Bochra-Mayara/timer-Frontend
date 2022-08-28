<template>
<!-- Header -->
<span class="date">Today,08 Oct</span>
 



 <input type="submit" value="Stop" class="button-Stop" />
 
 <input type="submit" value="Start new" class="button-Start" @click="show =! show" />

 <div v-if="show" class="container">
    <span> Period{{count}}</span>
   
    </div>
    




 <div class="total">
    <span >Total: </span>
 </div>   
 
 
</template>

<script>
import { ref, computed } from 'vue'
import store from '@/store/index'
export default {
    name: 'TrackerComponent',
    data() {
        return {
            show:false
        }
    },
  
    setup() {
        const tracking = computed(() => store.state.trackId !== null);
        const currentTracking = ref(null);
        const trackingTime = ref(null);

        var date=new Date();
       var hours=date.getHours();
       var minutes=date.getMinutes();
       var seconds=date.getSeconds();
        var day=date.getDay();
       var month=date.getMonth();
     
       
        


        const startTracking = async () => {
                    store.methods.startTracking(currentTracking.value.id);
                    console.log(store.state.trackId);
                    computeTracking();
        
            
        };

        const stopTracking = async () => {
            
                store.methods.stopTracking();
                currentTracking.value = null;
                trackingTime.value = null;
            
        }


        const dateTest = (date:Date) =>{
       
     console.log("Current Time: "+hours+":"+minutes+":"+seconds); 
     console.log("Date: "+day+":"+month); 

      }
           const increment=() =>{
               this.count++
               }



        const toTime = (ms) => {
            const seconds = Math.floor((ms / 1000) % 60);
            const minutes = Math.floor((ms / (1000*60)) % 60);
            const hours = Math.floor(((ms / (1000*60*60))) %24);
            return {hours, minutes, seconds};
        }

        const computeTracking = () => {
            const time = currentTracking.value ? new Date().getTime() - new Date(currentTracking.value.startTime).getTime() : null;
            if (time) {
                const {hours, minutes, seconds} = toTime(time);
                trackingTime.value = { 
                    hours: String(hours).padStart(2, '0'),
                    minutes: String(minutes).padStart(2, '0'),
                    seconds: String(seconds).padStart(2, '0')
                }
            }

            setTimeout(computeTracking, 1000);
        }
        if(tracking.value) {
            computeTracking();
        }

       

        return {
            dateTest,
            trackingTime,
            tracking,
            currentTracking,
            startTracking,
            stopTracking,
            increment,
        }
    }
}

    
    
        
      
       
</script>
<style lang="scss">

    .date{
   
   width: 183px;
   height: 35px;
   font-family: 'Roboto';
   font-style: normal;
   font-weight: 900;
   font-size: 30px;
   line-height: 35px;
   color: #415C73;
   margin-top: 800px;
   }
   .button-Stop{
     
     background-color: white;
     border: 2px solid #CC5252;
     color: #CC5252;
     cursor: pointer;
     width: 86px;
     height: 38px;
     font-weight: 900;
     font-size: 0.8rem;
    margin-top: 2px;
    margin-left: 90px;
   
   }
   .button-Stop:hover {
     background-color: #CC5252;
      color: white;
   
   }
   
   .button-Start{
     background-color: white;
     border: 2px solid #60B669;
     color: #60B669;
     cursor: pointer;
     width: 120px;
     height: 38px;
     font-weight: 900;
     font-size: 0.8rem;
     margin-top:2px;
     margin-left: 60px;
     
   }
   .button-Start:hover {
     background-color: #60B669;
      color: white;
   
   }
   .total{
     width: 5vw;
     margin-top: 30%;
    padding-left: 60%;
    position: fixed;
    
   }
   .container{
     box-shadow: 0 0 4px rgba(0, 0, 0, 0.5);
     border-radius: 15px;
     width:50%;
     height: 10vh;
     margin-left: 25%;
     flex-direction: column;
     justify-content: center;
     align-items: center;
     position: relative;
     top: 50px;
   
       
   }
   
   
   
   </style>
