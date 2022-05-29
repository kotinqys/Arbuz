<template>
<div class="info">
    <p>Ряд : {{watermelon.row}} Место: {{watermelon.column}}</p>
    <p>Количество aрбузов: {{watermelon.data.length}}</p>
    <div 
        class='cart' 
        v-for="(item,idx) in watermelon.data" 
        @click="()=>chooseCart(item.id,item.status)"
        v-bind:class="{ 'red': item.status == 'спелый','green': item.status == 'не спелый', 'gray': item.status == 'уже сорван','active':active[item.id]}">
            <p>Статус : {{item.status}}</p>
            <p>Вес : {{item.weight}} кг</p>
    </div>
</div>
</template>

<script>
export default{
    props:{
        watermelon:{
            type:Object
        },
        active:{
            type:Object,
            required:true
        }
    },
    methods:{
        chooseCart(id,status){
            if(status!=='уже сорван')
                this.$emit('chooseCart',id,)
        }
    }

}
</script>

<style scoped>
.active{
    border: 2px solid #000!important;
}
.info{
    display: flex;
    flex-direction: column;
    align-items: center;
}
p{
    font-weight: 500;
    font-size: 18px;
    margin: 7px;
}
.cart{
    cursor: pointer;
    width: 300px;
    border-radius: 2px;
    margin:5px 0px;
    border: 2px solid rgba(45, 186, 113, 0);;
}
.red{
    background-color: #d84861;
}
.green{
    background-color: rgb(45, 186, 112);
}
.gray{
    background-color: rgb(128, 128, 128);
}
.cart p{
    color:#fff
}
</style>