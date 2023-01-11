<template>
    <div class="container">
        <div class="elevators">
            <elevator :floors = "floors" :destinationFloor = "destinationFloor" :id="1" @update="checkElevator"/>
        </div>  
    </div>
    
</template>

<script>
    import Elevator from "@/components/Elevator.vue"

    export default {
        components:{
            Elevator,
        },
        data(){
            return {
                destinationFloor:1,
                floors:5,
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
}

body{
    margin: 0;
}

.container{
    padding: 2%;
    display: flex;
    align-items: center;
    height: 100vh;
}
</style>

