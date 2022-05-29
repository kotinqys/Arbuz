<template>
    <div class="title">
        <img src="../assets/watermelon.png" alt="watermelons" width="100" height="100">
        <h1>Заказ Арбузов</h1>
        <h3>Выберите арзбуз(ы) из грятки</h3>
    </div>
    <div class="watermelons">
        <WatermelonItem v-for="watermelon in watermelons" :watermelon="watermelon" @show="showWatermelon"/>
    </div>
    <div class="content">
        <WaterMelonCart :watermelon="selected" :active="active" @chooseCart="chooseCart" v-if="isSelected" />
        <button class="button" @click="order">Заказать</button>
        <Order v-if="isShow" @close="close" :data="selected" @onBuy="onBuy"/>
    </div>
</template>

<script>
import WatermelonItem from './WatermelonItem.vue'
import WaterMelonCart from './WaterMelonCart.vue'
import Order from './Order.vue';
export default {
    components: { WatermelonItem, WaterMelonCart, Order },
    data() {
        return {
            watermelons: [
                { id: 1, row:1,column:1,data: [{id:1, status: "спелый", weight: 4 },{ id:2, status: "спелый", weight: 4 }, { id:3,status: "спелый", weight: 2 }] },
                { id: 2, row:1,column:2,data: [{id:4, status: "спелый", weight: 2 }, {id:5, status: "спелый", weight: 2 }] },
                { id: 3, row:1,column:3,data: [{id:6, status: "уже сорван", weight: 1 }, {id:7, status: "уже сорван", weight: 2 }] },
                { id: 4, row:1,column:4,data: [{id:8, status: "спелый", weight: 5 }, {id:9, status: "спелый", weight: 2 }] },
                { id: 5, row:1,column:5,data: [{id:10, status: "не спелый", weight: 1 }, {id:11, status: "не спелый", weight: 2 }] },
                { id: 6, row:1,column:6,data: [{id:12, status: "спелый", weight: 4 }, {id:13, status: "спелый", weight: 2 }] },
                { id: 7, row:1,column:7,data: [{id:14, status: "спелый", weight: 2 }, {id:15, status: "спелый", weight: 2 }] },
                { id: 8, row:1,column:8,data: [{id:16, status: "уже сорван", weight: 1 },{id:17, status: "уже сорван", weight: 1 }, {id:18, status: "уже сорван", weight: 2 }] },
                { id: 1, row:2,column:1,data: [{id:19, status: "спелый", weight: 5 }] },
                { id: 10, row:2,column:2,data: [{id:20, status: "не спелый", weight: 1 }, {id:21, status: "не спелый", weight: 2 }] },
                { id: 11, row:2,column:3,data: [{id:22, status: "спелый", weight: 4 },{id:23, status: "уже сорван", weight: 1 }, {id:24, status: "спелый", weight: 2 }] },
                { id: 12, row:2,column:4,data: [{id:25, status: "спелый", weight: 2 }, {id:26, status: "спелый", weight: 2 }] },
                { id: 13, row:2,column:5,data: [{id:27, status: "уже сорван", weight: 1 }, {id:28, status: "уже сорван", weight: 2 }] },
                { id: 14, row:2,column:6,data: [{id:29, status: "спелый", weight: 5 }, {id:30, status: "спелый", weight: 2 }] },
                { id: 15, row:2,column:7,data: [{id:31, status: "не спелый", weight: 1 }, {id:32, status: "не спелый", weight: 2 }] },
                { id: 16, row:2,column:8,data: [{id:33, status: "спелый", weight: 4 }] },
                { id: 17, row:3,column:1,data: [{id:34, status: "спелый", weight: 2 },{id:35, status: "спелый", weight: 5 }, {id:36, status: "спелый", weight: 2 }] },
                { id: 18, row:3,column:2,data: [{id:37, status: "уже сорван", weight: 1 }, {id:38, status: "уже сорван", weight: 2 }] },
                { id: 19, row:3,column:3,data: [{id:39, status: "спелый", weight: 5 }, {id:40, status: "спелый", weight: 2 }] },
                { id: 20, row:3,column:4,data: [{id:41, status: "не спелый", weight: 2 }] },
                { id: 21, row:3,column:5,data: [{id:42, status: "не спелый", weight: 2 }] },
                { id: 22, row:3,column:6,data: [{id:43, status: "не спелый", weight: 2 }] },
                { id: 23, row:3,column:7,data: [{id:44, status: "уже сорван", weight: 2 }] },
                { id: 24, row:3,column:8,data: [{id:45, status: "спелый", weight: 2 }] },
            ],
            isSelected:false,
            selected:null,
            isShow:false,
            active:{}
        };
    },
    methods:{
        showWatermelon(watermelon){
            this.isSelected = true
            this.selected = watermelon
        },
        order(){
            if(this.selected !== null && Object.keys(this.active).length !== 0){
                this.isShow = true
            }else{
                alert("Выберите арбуз!")
            }
        },
        close(){
            this.isShow = false
        },
        onBuy(data){
            this.watermelons = this.watermelons.map((watermelon)=>{
                if(data.id==watermelon.id){
                    return {
                        ...watermelon,
                        data: watermelon.data.map((val=>(
                            {...val,status:val.status = 'уже сорван'}
                        )))
                    }
                }
                this.active={}
                return watermelon
            })
        },
        chooseCart(id){
            if(this.active[id]){
                let state = {...this.active}
                delete state[id]
                this.active = state
            }else{
                this.active = {...this.active,[id]:true}
            }
        }
    }
}
</script>

<style scoped>
.watermelons{
    max-width: 600px;
    margin: 0 auto;
    display: grid;
    grid-template-columns: repeat(8, 1fr);
}
.content{
    text-align: center;
    margin-top: 10px;
}
.title{
    text-align: center;
}
h2{
    color: rgb(39, 39, 39);
    letter-spacing: 0.7px;
}
.button{
    cursor: pointer;
    font-size: 20px;
    width: 340px;
    border: none;
    margin-top: 10px;
    background-color: #399349;
    color: #fff;
    border-radius: 5px;
    padding: 10px 50px;
}
</style>