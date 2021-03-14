<template>
    <div>
        <ion-item v-if="error.length > 0">
            <ion-grid>
                <ion-col>
                    <ion-text v-for="error in error" :key="error" class="danger">
                        <p class="ion-text-center">{{ error }}</p>
                    </ion-text>
                </ion-col>
            </ion-grid>
        </ion-item>
        <ion-item v-if="jsonMessage">
            <ion-grid>
                <ion-col>
                    <ion-text class="warning">
                        <p class="ion-text-center">{{ jsonMessage }}</p>
                    </ion-text>
                </ion-col>
            </ion-grid>
        </ion-item>
        <ion-item>
            <ion-label>Date de réservation</ion-label>
            <ion-datetime 
                displayFormat="DD/MM/YYYY"
                v-model="date"
                :min="todayDate"
                max="2099"
                :value="todayDate"
            ></ion-datetime>
        </ion-item>
        <ion-item>
          <ion-label position="floating">Heure</ion-label>
          <ion-input v-model="hour" name="hour" type="number"></ion-input>
        </ion-item>
        <ion-item>
          <ion-label position="floating">Adresse mail</ion-label>
          <ion-input v-model="email" name="email" type="email"></ion-input>
        </ion-item>
        <ion-item>
            <ion-label>Accepter les CGU</ion-label>
            <ion-checkbox v-model="cgu" color="primary"></ion-checkbox>
        </ion-item>

        <ion-button expand="block" @click="reserver" >Réserver</ion-button>
    </div>
</template>

<script>
    import { IonItem, IonLabel, IonInput, IonButton, IonDatetime, IonCheckbox, IonText, IonGrid, IonCol } from '@ionic/vue' 

    export default {
        name: "Reservation",
        components: { IonItem, IonLabel, IonInput, IonButton, IonDatetime, IonCheckbox, IonText, IonGrid, IonCol },
        data() {
            return {
                todayDate: '',
                date: '',
                email: '',
                hour: '',
                error: [],
                jsonMessage: '',
                uniqueId: '',
                cgu: false,
            }
        },
        mounted(){
            this.getDate()
        },
        methods: {
            getDate(){
                var today = new Date() 
                if(today.getMonth()+1 < 10 && today.getDate() < 10 ){
                    this.todayDate = `${today.getFullYear()}-0${today.getMonth()+1}-0${today.getDate()}`
                }else if(today.getMonth()+1 > 9 && today.getDate() < 10 ){
                    this.todayDate = `${today.getFullYear()}-${today.getMonth()+1}-0${today.getDate()}`
                }else if(today.getMonth()+1 < 10 && today.getDate() > 9 ){
                    this.todayDate = `${today.getFullYear()}-0${today.getMonth()+1}-${today.getDate()}`
                }else if(today.getMonth()+1 > 9 && today.getDate() > 9 ){
                    this.todayDate = `${today.getFullYear()}-${today.getMonth()+1}-${today.getDate()}`
                }
                console.log(this.todayDate)
            },
            validateEmail(email) {
                const regex = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/ 
                return regex.test(String(email).toLowerCase()) 
            },
            reserver(){
                var newDate = new Date(this.date)
                let newDateString = ""
                if(newDate.getMonth()+1 < 10 && newDate.getDate() < 10 ){
                    newDateString = `${newDate.getFullYear()}-0${newDate.getMonth()+1}-0${newDate.getDate()}`
                }else if(newDate.getMonth()+1 > 9 && newDate.getDate() < 10 ){
                    newDateString = `${newDate.getFullYear()}-${newDate.getMonth()+1}-0${newDate.getDate()}`
                }else if(newDate.getMonth()+1 < 10 && newDate.getDate() > 9 ){
                    newDateString = `${newDate.getFullYear()}-0${newDate.getMonth()+1}-${newDate.getDate()}`
                }else if(newDate.getMonth()+1 > 9 && newDate.getDate() > 9 ){
                    newDateString = `${newDate.getFullYear()}-${newDate.getMonth()+1}-${newDate.getDate()}`
                }

                this.uniqueId = Math.random().toString(36).substring(2, 15) + Math.random().toString(36).substring(2, 15) 

                this.error = [] 
                this.jsonMessage = '' 
                if(!this.cgu){
                    this.error.push("Veuillez accepter les CGU.") 
                } 
                if(!this.validateEmail(this.email)){
                    this.error.push("Veuillez entrer un email valide.") 
                } 
                if(this.hour > 18 || this.hour < 9){
                    this.error.push("Vous ne pouvez réserver que de 9 à 17h")
                }
                if(this.error.length > 0){
                    return
                } 
                let data = {
                    date: newDateString,
                    email: this.email,
                    uniqueId: this.uniqueId,
                    hour: this.hour
                } 
                fetch('https://laravel-reservation.herokuapp.com/api/booking', {
                    method: 'POST',
                    body: JSON.stringify(data),
                        headers: {
                            'Accept': 'application/json',
                            'Content-Type': 'application/json'
                        },
                })
                .then(res => res.json())
                .then(data => {
                    if(data){
                        this.jsonMessage = data
                    }
                })
            },
        },
    }
</script>

<style scoped>
    .warning{
        color: orange;
    }
</style>
