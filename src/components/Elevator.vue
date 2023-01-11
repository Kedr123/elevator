<template>
        <div class="mine">
            <div class="elevator" :style="{
                'top': 'calc( 100% - calc(calc(100%/'+floors+')*'+position+'))',
                 'transition-duration': duration+'s',
                 'animation': isAnimation ? 'mer 0.75s linear infinite': ''
                 }">
                
                <div class="indicator" v-if="indicatorDown"> - {{destinationFloor}}</div>
                <div class="indicator" v-if="indicatorUp"> + {{destinationFloor}}</div>
                
            </div>
        </div>
</template>

<script>
export default {
    props:{
        floors:{
            // type: string,
            // required: true
        },
        destinationFloor:{},
        id:{},
        
        
    },
    data(){
        return { 
            position: 1,
            duration:1,
            isAnimation:false,
            indicatorDown:false,
            indicatorUp:false,
        }
    },
    methods:{
        toFloor(floor){
            this.duration = floor-this.position;
            if( this.duration<0){
                this.duration = this.duration * -1;
                this.indicatorDown = true;
            }
            else{
                this.indicatorUp = true;
            }    

            this.position = floor;

            setTimeout(()=>{

                this.isAnimation = true;

                setTimeout(()=>{
                    this.indicatorDown = false;
                    this.indicatorUp = false;
                    this.isAnimation = false;
                    this.$emit('update', this.id);

                }, 3000);

            }, this.duration*1000);
            
        }
    },
    watch:{
        destinationFloor(newValue){
            console.log(newValue);
            this.toFloor(newValue);
        }
    }
}
</script>

<style>
    .mine{
        height: 90%;
        width: clamp(100px,calc(100%/5),150px);
        position: absolute;
    }
    .elevator{
        position: relative;
        height: calc(100%/5);
        width: 100%;
        background-color: aqua;
        opacity: 1;
        transition-timing-function: linear;
        display: flex;
        justify-content: center;
    }

    .indicator{
        border-radius: 5px;
        margin-top: 3%;
        padding: 2% 10%;
        color: white;
        background-color: #c4c4c4;
        height: 15%;

        display: flex;
        align-items: center;
        justify-content: center;
    }

    @keyframes mer{
        50%{
            opacity: 0;
        }
    }

</style>