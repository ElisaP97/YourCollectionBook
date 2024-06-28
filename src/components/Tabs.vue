<script setup>

    //componenti importati 
    import Home from "./Home.vue";
    import Collection from "./Collection.vue";
    import Shoppinglist from "./Shoppinglist.vue";
    //librerie e funzioni importate
    import { ref } from 'vue';

    const tab = ref(1);
</script>

<template>
<div class="w-full">
    <!-- Tasti per switchare il contenuto -->
    <div class="flex justify-center p-4">
        <button v-on:click="tab=1" v-bind:class="{'btn-primary ' : tab===1}" class="btn rounded-full btn-s mx-1 sm:btn-sm md:btn-md lg:btn-lg lg:mx-2">Home</button> <!-- Forma intera-->
        <button @click="tab=2" :class="{'btn-secondary' : tab===2}" class="btn rounded-full btn-s mx-1 sm:btn-sm md:btn-md lg:btn-lg lg:mx-2">Collezione</button> <!-- Forma contratta-->
        <button @click="tab=3" :class="{'btn-accent' : tab===3}" class="btn rounded-full btn-1 mx-2 sm:btn-sm md:btn-md lg:btn-lg lg:mx-2">Uscite da comprare</button>
    </div>

    <!--Contenuto-->
    <div>
        <div v-if="tab===1" class="py-10 px-6 ">
            <Home/>
        </div>
        <div v-if="tab===2" class="py-10 px-6">
            <Collection/>
        </div>
        <div v-if="tab===3" class="py-10 px-6">
            <Shoppinglist/>
        </div>
    </div>
</div>
</template>

<style scoped>
</style>


