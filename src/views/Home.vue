<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title class="ion-text-center">Accueil</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">Accueil</ion-title>
        </ion-toolbar>
      </ion-header>
      <Informations v-model:infos="infos" />
      <InformationsDays v-model:infos="infos" />
    </ion-content>
  </ion-page>
</template>

<script>
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent } from '@ionic/vue'
import InformationsDays from '@/components/InformationsDays'
import Informations from '@/components/Informations'

export default  {
  name: 'Home',
  components: { IonHeader, IonToolbar, IonTitle, IonContent, IonPage, InformationsDays, Informations },
  async created() {
    await fetch('https://laravel-reservation.herokuapp.com/api/infos')
    .then(res => res.json())
    .then(data => {
        this.infos = data
        })
  },
  data() {
    return {
      infos: ""
    }
  },
}
</script>
