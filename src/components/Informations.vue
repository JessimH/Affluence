<template>
    <div class="informations">
        <p v-if="infos">
            <span class="text">Etablissement :</span> 
            {{ infos.title }}
        </p>
        <p v-if="infos">
            <span class="text">Temps de réservation :</span> 
            {{ infos.durée_reservation_seconde / 3600 }} h
        </p>
        <p v-if="infos">
            <span class="text">Limite de réservations par heures : </span> 
            {{ infos.limite_reservation_max }}
        </p>
    </div>
</template>

<script>
    export default {
        props: [ 'infos' ]
    }
</script>

<style scoped>
    .informations {
        width:100%;
        display: flex;
        align-items: center;
        flex-direction: column;
    }
    .text{
        font-weight: bold;
    }
</style>