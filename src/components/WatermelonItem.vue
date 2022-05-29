<template>
    <div class="watermelon" @click="openWatermelon">
        <img src="../assets/watermelon2.png" alt="" width="55" height="55">
    </div>
</template>

<script>
export default{
    props:{
        watermelon:{
            type:Object,
            required:true
        }
    },
    methods:{
        openWatermelon(){
            this.$emit("show",this.watermelon)
        }
    }
}
</script>

<style scoped>
.watermelon{
    cursor: pointer;
    width: 50px;
    height: 50px;
    margin: 10px;
}
</style>