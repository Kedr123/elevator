<template>
    <div class="floors" >
        <button  v-for="i in floors" class="button" :class="{active:queue.includes(i)}">
            {{ i }}
        </button>
    </div>
</template>

<script>
export default {
    props:{
        floors:{},
        queue:{}
    },
    data(){
        return{

        }
    },
    methods:{
        buttonClick(id){
            if(queue.includes(id)){
                return;
            }

            this.$emit("append", id)
        }
    }
}
</script>

<style scoped>
    .floors{
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: space-around;
        height: 90%;
        margin-left: 3%;
    }

    .button{
        padding: 50%;
        border-radius: 50%;
        width: 30px;
        height: 30px;

        display: flex;
        justify-content: center;
        align-items: center;

        background-color: white;
        border: 2px solid blue;
    }

    .active{
        background-color: red;
    }

</style>