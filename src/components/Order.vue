<template>
<div class="order">
    <div class="body">
        <span @click="onClose">X</span>
        <label>Адрес доставки :</label>
        <p class="error" v-if="errorLocation">*Обязательное поле</p>
        <input type="text" placeholder="г.Алматы, улица Абая 66" v-model="location">
        <label>Номер телефона : </label>
        <p class="error" v-if="errorPhone">*Обязательное поле</p>
        <input type="number" placeholder="+7 777 77 77" v-model="phone">
        <label>Дата доставки </label>
        <p class="error" v-if="errorCurrentDate">*Обязательное поле</p>
        <div class="dates">
            <div 
                class ="date" 
                v-for="date in dates" 
                @click="()=>setDate(date)" 
                v-bind:class="{ 'active': currentDate==date}">
                    {{date}}
            </div>
        </div>
        <label>Время доставки</label> 
        <p class="error" v-if="errorTime">*Обязательное поле</p>
        <input type="time" v-model="time">
        <div class="checkbox">
            <input type="checkbox" v-model="isCated">
            <label>Порезать дольками</label>
        </div>
        <button class="button" @click="onBuy">Оформить заказ</button>
    </div>
</div>
</template>

<script>
export default{
    props:{
        data:{
            type:Object,
            required:true
        }
    },
    data(){
        return {
            dates:[],
            location:"",
            phone:"",
            currentDate:"",
            time:"",
            isCated:false,
            errorLocation:false,
            errorCurrentDate:false,
            errorPhone:false,
            errorTime:false,
        }
    },
    methods:{
        onClose(){
            this.$emit('close')
        },
        setDate(date){
            this.currentDate = date
        },
        onBuy(){
            this.validate()
            if(this.location.length >= 8 && this.phone.length >= 8 && this.currentDate !== "" && this.time!== ""){
                this.$emit("onBuy",this.data)
                alert("Заказ успешно принят!!!")
                this.onClose()
            }
        },
        validate(){
            if(this.location.length < 8)
                this.errorLocation = true
            else
                this.errorLocation = false
            if(this.phone.length < 8 )
                this.errorPhone = true  
            else
                this.errorPhone = false 
            if(this.currentDate=="")
                this.errorCurrentDate = true  
            else
                this.errorCurrentDate = false  
            if(this.time=="")
                this.errorTime = true  
            else
                this.errorTime = false 
        }
    },
    mounted(){
        const monthes=['Января','Февраля','Марта','Апреля','Мая','Июня','Июля','Августа','Сентября','Ноября','Декабря'];
        const date = new Date();
        let dd = date.getDate()
        let mm = monthes[date.getMonth()];
        let lastDayDate = new Date(date.getFullYear(), date.getMonth() + 1, 0);
        let lastDay = lastDayDate.toLocaleString('RU', {day: 'numeric'});
        for(let i=0;i<=9;i++){
            dd+=1
            let today = dd+ " " + mm
            if(dd>lastDay){
                dd = 1
                mm = monthes[date.getMonth()+1];
                today = dd + " " + mm
            }
            this.dates.push(today)
        }
    }
}
</script>

<style scoped>
input[type="number"]::-webkit-outer-spin-button,
input[type="number"]::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}
.error{
    color: rgb(190, 10, 10);
    margin: 0;
    padding: 0;
}
.order{
    position: absolute;
    top:0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.384);
    display: flex;
    justify-content: center;
    align-items: center;
}
.body{
    padding: 50px;
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    position: fixed;
    background-color: rgb(255, 255, 255);;
    border-radius: 5px;
}
.dates{
    width: 500px;
    margin: 0 auto;
    display: grid;
    grid-template-columns: repeat(4,1fr);
    justify-content: center;
}
.date{
    cursor: pointer;
    font-size: 18px;
    background-color: #399349;
    color: #fff;
    border-radius: 4px;
    padding: 10px;
    margin: 5px;
    border: 2px solid #399349;
}

.active{
    border: 2px solid #000;
}
.button{
    align-self: flex-end;
    cursor: pointer;
    font-size: 20px;
    border: none;
    margin-top: 10px;
    background-color: #399349;
    color: #fff;
    border-radius: 5px;
    padding: 10px 50px;
}
.checkbox{
    margin-top: 20px;
    font-weight: 600;
}
input{
    padding: 7px;
    outline: none;
}
label{
    margin: 20px 0px 5px 0px;
    font-weight: 600;
}
span{
    cursor: pointer;
    position: absolute;
    top:10px;
    right: 20px;
    font-weight: 700;
}
</style>