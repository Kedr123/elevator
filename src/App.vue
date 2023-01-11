<template>
    <div class="container">
        <div class="elevators" :style="{'width': 'calc(clamp(100px,calc(100%/5),150px)*'+countElevators+')'}">
            <elevator v-for="i in countElevators"
             v-on:vnodeMounted="()=>{statusElevators.push(false); destinationFloor.push(1) }" 
             v-on:vnodeUnmounted="()=>{statusElevators.pop(); destinationFloor.pop()}"  
             :floors = "floors" 
             :destinationFloor = "destinationFloor[i-1]" 
             :id="i-1"
             :key="i-1"
             @update="checkElevator"/>
           
        </div> 
        <floors :floors = "floors" :queue = "queue" @elevatorСall="elevatorСall"/>  
    </div>
    
</template>

<script>
    import Elevator from "@/components/Elevator.vue";
    import Floors from "@/components/Floors.vue";

    export default {
        components:{
            Elevator, Floors,
        },
        beforeMount(){
            if(this.isSave){
                this.getCountElevators();
                
            }           
        },
        mounted(){
            this.getDestinationFloor();
            this.get();
            this.nextCall();
        },
        data(){
            return {
                // Количество лифтов
                countElevators:1,
                // Количество этажей
                floors:5,
                // Включить/выключить сохранение
                isSave:false,
                // включить/выключить приоритет для ближайшего свободного лифта
                isPriority:false,

                statusElevators:[],
                destinationFloor:[],
                queue:[],
                queueElevators:[],
            }
        },
        methods:{
            // Вызывается, когда лифт выполнил текущие задание
            checkElevator(id,position){
                this.statusElevators[id]=false;
                
                if(this.queue.indexOf(position)>=0) this.queue.splice(this.queue.indexOf(position), 1);

                
                this.nextCall();
            },

            // Вызывается при нажатии на кнопку
            elevatorСall(id){
                if(this.destinationFloor.includes(id)){
                    return;
                }
                
                this.queue.push(id);
                this.queueElevators.push(id);
                
                this.nextCall();
            },

            // Функция для нахождения свободного лифта и выдачи ему нового задания
            nextCall(){
                this.save(); 
                if(this.queueElevators.length!=0){
                    // Простой вариант - приоритет всегда у свободного лифта с наименьшим индексом в массиве
                    if(!this.isPriority){
                        for(let i = 0; i< this.countElevators; i++){
                        
                            if(this.statusElevators[i]==false){
                                this.destinationFloor[i] = this.queueElevators.shift();
                                this.statusElevators[i]=true;
                                this.save(); 
                                return;
                            }    
                        }
                    }
                    // Усложнённый вариант - приоритет у ближайшего свободного лифта 
                    else{
                        
                        let floor = this.queueElevators.shift();
                        let nearestElevator;

                        for(let i = 0; i< this.countElevators; i++){
                        
                            if(this.statusElevators[i]==false){
                                 
                                if(nearestElevator==undefined){
                                    nearestElevator = i;
                                }
                                else if((floor - this.destinationFloor[nearestElevator])**2>(floor - this.destinationFloor[i])**2) {
                                    nearestElevator = i;
                                }

                            }
                        
                        }

                        this.destinationFloor[nearestElevator]=floor;
                        this.statusElevators[nearestElevator]=true;

                    } 
                    this.save(); 
                }
            }, 
                

        

            // Функция для сохранения данных. Вызывается при изменении зависимостей, отслеживаемых через watch
            save(){
                if(this.isSave){
                    localStorage.setItem('countElevators', this.countElevators);
                    localStorage.setItem('destinationFloor', this.destinationFloor);
                    localStorage.setItem('floors', this.floors);
                    localStorage.setItem('statusElevators', this.statusElevators);
                    localStorage.setItem('queue', this.queue);
                    localStorage.setItem('queueElevators', this.queueElevators);
                }
            },

            // Функция для получения сохранённых данных, вызывается после монтирования элемента App
            get(){
                localStorage.getItem('floors')?this.floors=Number(localStorage.getItem('floors')):false;
                localStorage.getItem('statusElevators')?this.statusElevators = localStorage.getItem('statusElevators').split(','):false;
                localStorage.getItem('queue')?this.queue = localStorage.getItem('queue').split(',').map(Number):false;
                localStorage.getItem('queueElevators')?this.queueElevators = localStorage.getItem('queueElevators').split(',').map(Number):false;
            },
            getDestinationFloor(){
                localStorage.getItem('destinationFloor')?this.destinationFloor = localStorage.getItem('destinationFloor').split(',').map(Number):false;
            },
            getCountElevators(){
                localStorage.getItem('countElevators')?this.countElevators=Number(localStorage.getItem('countElevators')):false;
            }
        
        },
        watch:{
            countElevators(newValue){
                localStorage.setItem('countElevators', this.countElevators);
            },            

            floors(newValue){
                localStorage.setItem('floors', this.floors);
            },                      

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
    margin-left: 2%;
    display: flex;
    align-items: center;
    height: 100vh;
}
</style>

