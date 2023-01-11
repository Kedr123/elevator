<template>
        <div class="mine">
            
            <div class="elevator" :class="{anime:isAnimation}" :style="{
                'top': 'calc( 100% - calc(calc(100%/'+floors+')*'+(position)+'))',
                 'transition-duration': duration+'s',
                 'height': 'calc(calc(100%/'+floors+') - 4px)',                 
                 }">
                
                <div class="indicator" v-if="indicatorDown"> &#129047; {{destinationFloor}}</div>
                <div class="indicator" v-if="indicatorUp"> &#129045; {{destinationFloor}}</div>
                
            </div>
            <table>
                <tr v-for="i in floors"><td></td></tr>
            </table>
            
        </div>
</template>

<script>
export default {
    props:{
        floors:{},
        destinationFloor:{
            default:1
        },
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
                    this.$emit('update', this.id, this.position);

                }, 3000);

            }, this.duration*1000);
            
        }
    },
    watch:{
        destinationFloor(newValue){
            if(newValue){
            this.toFloor(newValue);
            }
        }
    }
}
</script>

<style scoped>

    .anime{
        animation: mer 0.75s linear infinite;
    }
    .mine{
        height: 90%;
        width: clamp(100px,calc(100%/5),150px);
        border: 1px solid black;
        position: relative;
    }
    table{
        position: relative;
        border: 1px solid #c4c4c4;
        width: 100%;
        height: 100%;
        z-index: 0;
        top:0;
    }
    td{
        border: 1px solid #c4c4c4; 
    }
    .elevator{
        position: absolute;
        
        width: calc(100% - 8px);
        margin-left: 4px;

        background-color: rgb(204, 219, 172);
        opacity: 1;
        transition-timing-function: linear;
        display: flex;
        justify-content: center;
        margin-top: 2px;
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