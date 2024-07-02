<script setup>
import Collection from "./Collection.vue";
import tab from "./Tabs.vue";

import { ref } from 'vue';

const Go = ref(1);
</script>

<template>
<!-- div contenitore della pagina home -->
<div v-if="Go===1" class="flex md:container md:mx-auto h-screen justify-center rounded">
    <div class="hero bg-neutral h-3/4 sm:4/5 md:w-4/5 " style="background-image: url(../../public/manga.png)">
        <div class="hero-overlay bg-primary-content h-full w-full bg-opacity-80"></div>
        <div class="hero-content text-center">
            <div class="max-w-md">
                <h1 class="text-5xl font-bold">Benvenuto nella tua libreria</h1>
                <p class="text-2xl py-6">
                    Qui puoi tener traccia della tua collezione, dei fumetti che hai letto e non, di quelli in corso e di quelli completi, oltre alla possibilità di tenere una lista delle uscite ancora da comprare.
                </p>
                    <button @click="Go=2,tab=2" class="btn btn-primary">Lesgoski</button>
            </div>
        </div>
    </div>
</div>
<div v-if="Go!=1"><Collection/></div>
</template>


<style scoped>

</style>
