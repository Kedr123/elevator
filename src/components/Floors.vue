<template>
    <div class="floors" >
        <button  v-for="i in floors"  :class="{active:queue.includes(i), button:!queue.includes(i) }" :key="i" :id="i" @click="buttonClick($event)">
            {{ i }}
        </button>
    </div>
</template>

<script>
export default {
    props:{
        floors:{},
        queue:{},

    },
    data(){
        return{

        }
    },
    methods:{
        buttonClick(event){
            if(this.queue.includes(Number(event.target.id))){
                return;
            }
            
            this.$emit("elevatorСall", Number(event.target.id))
        }
    }
}
</script>

<style scoped>
    .floors{
        display: flex;
        flex-direction: column-reverse;
        align-items: center;
        justify-content: space-around;
        height: 90%;
        margin-left: 3%;
    }

    .button, .active{
        padding: 50%;
        border-radius: 50%;
        width: 50px;
        height: 50px;
        font-size: 22px;

        display: flex;
        justify-content: center;
        align-items: center;

        background-color: #f8f8f8;
        border: 2px solid #d0d0d0;
        color: #898989;
    }

    .button:hover{
        background-color: #dfeff4;
        border: 2px solid #9dd6e6;
        color: #738aac;
    }

    .active{
        background-color: #f4e8df;
        border: 2px solid #e6c49d;
        color: #ac8c73;
    }

</style>