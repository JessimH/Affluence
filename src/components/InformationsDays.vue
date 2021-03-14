<template>
    <div>
        <ion-grid v-if="infos" class="ion-text-center">
            <ion-row>
                <ion-col>
                    Lundi
                </ion-col>
                <ion-col>
                        {{ infos.lundi }}
                </ion-col>
            </ion-row>
            <ion-row>
                <ion-col>
                    Mardi
                </ion-col>
                <ion-col>
                        {{ infos.mardi }}
                </ion-col>
            </ion-row>
            <ion-row>
                <ion-col>
                    Mercredi
                </ion-col>
                <ion-col>
                        {{ infos.mercredi }}
                </ion-col>
            </ion-row>
            <ion-row>
                <ion-col>
                    Jeudi
                </ion-col>
                <ion-col>
                        {{ infos.jeudi }}
                </ion-col>
            </ion-row>
            <ion-row>
                <ion-col>
                    Vendredi
                </ion-col>
                <ion-col>
                        {{ infos.vendredi }}
                </ion-col>
            </ion-row>
            <ion-row>
                <ion-col>
                    Samedi
                </ion-col>
                <ion-col>
                        {{ infos.samedi }}
                </ion-col>
            </ion-row>
            <ion-row>
                <ion-col>
                    Dimanche
                </ion-col>
                <ion-col>
                       {{ infos.dimanche }}
                </ion-col>
            </ion-row>
        </ion-grid>
    </div>
</template>

<script>
    import { IonGrid, IonCol, IonRow } from '@ionic/vue'

    export default {
        name: "InformationsDays",
        components: { IonGrid, IonCol, IonRow },
        props: [ 'infos' ]
    }
</script>
