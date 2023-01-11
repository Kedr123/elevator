<template>
    <div class="container">
        <div class="elevators">
            <elevator :floors = "floors" :destinationFloor = "destinationFloor" :id="1" @update="checkElevator"/>
           
        </div> 
        <floors :floors = "floors" :queue = "queue"/>  
    </div>
    
</template>

<script>
    import Elevator from "@/components/Elevator.vue";
    import Floors from "@/components/Floors.vue";

    export default {
        components:{
            Elevator, Floors,
        },
        data(){
            return {
                destinationFloor:1,
                floors:5,
                queue:[]
            }
        },
        methods:{
            checkElevator(id){
                console.log("лифт - "+id)
            }
        }
    }
</script>

<style >

.elevators{
    display: flex;
    height: 100%;
    align-items: center;
    width: clamp(100px,calc(100%/5),150px);
}

body{
    margin: 0;
}

.container{
    /* padding: 2%; */
    margin-left: 2%;
    display: flex;
    align-items: center;
    height: 100vh;
    /* width: 100vw; */
}
</style>

