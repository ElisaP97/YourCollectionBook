<script setup>
    //componenti importati 

    //librerie e funzioni importate
    import axios from 'axios';
    import { onMounted } from 'vue';
    import { ref } from 'vue';
    import { computed } from 'vue';

    //logica

    const urlCollection= "http://localhost:3000/mangacollection";
    const books = ref([]);

    onMounted(()=> {
        axios.get(urlCollection).then(resp=>{
            books.value=resp.data;
        })
    })

    function showAll(){
        axios.get(urlCollection).then(resp=>{
        books.value=resp.data;
        })
    }

    function showNotRead(){
        axios.get(urlCollection).then(resp=>{
        let notRead = resp.data.filter((p) => p.reading === 3);
        books.value= notRead;
        })
    }

    function showCompletedRead(){
        axios.get(urlCollection).then(resp=>{
        let completedRead = resp.data.filter((p) => p.reading === 1);
        books.value= completedRead;
        })
    }

    function showReading(){
        axios.get(urlCollection).then(resp=>{
        let reading = resp.data.filter((p) => p.reading === 2);
        books.value= reading;
        })
    }


</script>

<template>
<!-- div contenitore della pagina collezione -->
<div class="flex md:container md:mx-auto h-screen justify-center">
    <!-- div contenitore con immagine manga -->
    <div class="bg-secondary-content h-3/4 w-full sm:w-4/5 md:w-4/5" style="background-image: url(../../public/manga.png)">
        <!-- div contenitore barra di ricerca e lista -->
        <div class="bg-primary-content h-full w-full bg-opacity-80 flex flex-wrap justify-center content-start gap-1 lg:gap-5 ">
            <div class="w-full md:w-4/5 mt-5 flex flex-wrap gap-3 justify-evenly">
                <button @click="showAll()" class="btn btn-xs lg:btn-md  btn-outline btn-secondary bg-secondary-content ">Tutta la collezione</button>
                <button @click="showNotRead()" class="btn btn-xs lg:btn-md  btn-outline btn-secondary bg-secondary-content ">
                    Da leggere
                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="size-6">
                        <path stroke-linecap="round" stroke-linejoin="round" d="M16.5 10.5V6.75a4.5 4.5 0 1 0-9 0v3.75m-.75 11.25h10.5a2.25 2.25 0 0 0 2.25-2.25v-6.75a2.25 2.25 0 0 0-2.25-2.25H6.75a2.25 2.25 0 0 0-2.25 2.25v6.75a2.25 2.25 0 0 0 2.25 2.25Z" />
                    </svg>
                </button>
                <button @click="showReading()" class="btn btn-xs lg:btn-md  btn-outline btn-secondary bg-secondary-content ">
                    Lettura in corso
                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="size-6">
                        <path stroke-linecap="round" stroke-linejoin="round" d="M17.593 3.322c1.1.128 1.907 1.077 1.907 2.185V21L12 17.25 4.5 21V5.507c0-1.108.806-2.057 1.907-2.185a48.507 48.507 0 0 1 11.186 0Z" />
                    </svg>
                </button>
                <button @click="showCompletedRead()" class="btn btn-xs lg:btn-md  btn-outline btn-secondary bg-secondary-content ">
                    Compleatati 
                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="size-6">
                        <path stroke-linecap="round" stroke-linejoin="round" d="M9 12.75 11.25 15 15 9.75M21 12c0 1.268-.63 2.39-1.593 3.068a3.745 3.745 0 0 1-1.043 3.296 3.745 3.745 0 0 1-3.296 1.043A3.745 3.745 0 0 1 12 21c-1.268 0-2.39-.63-3.068-1.593a3.746 3.746 0 0 1-3.296-1.043 3.745 3.745 0 0 1-1.043-3.296A3.745 3.745 0 0 1 3 12c0-1.268.63-2.39 1.593-3.068a3.745 3.745 0 0 1 1.043-3.296 3.746 3.746 0 0 1 3.296-1.043A3.746 3.746 0 0 1 12 3c1.268 0 2.39.63 3.068 1.593a3.746 3.746 0 0 1 3.296 1.043 3.746 3.746 0 0 1 1.043 3.296A3.745 3.745 0 0 1 21 12Z" />
                    </svg>
                </button>
                <button class="btn btn-xs lg:btn-md  btn-outline btn-secondary bg-secondary-content ">
                    Aggiungi Serie
                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="size-6">
                        <path stroke-linecap="round" stroke-linejoin="round" d="M12 9v6m3-3H9m12 0a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z" />
                    </svg>
                </button>
            </div>
            <div class="bg-secondary-content border border-secondary border-4 w-4/5 h-3/5 2xl:h-3/4 mt-5 p-5">
                <div class="overflow-auto h-full">
                    <table class="table">
                        <!-- head -->
                        <thead class="table-fixed">
                            <tr>
                                <th>Stato lettura</th>
                                <th>Nome e autore</th>
                                <th>Numero volumi</th>
                                <th>Status</th>
                                <th>Raiting</th>
                                <th></th>
                            </tr>
                        </thead>
                        <tbody v-for="book in books" :key="book.id">
                        <!-- row 1 -->
                        <tr>
                            <td>
                                <span v-if="book.reading===1"> <!--Letta tutta-->
                                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="size-6">
                                <path fill-rule="evenodd" d="M8.603 3.799A4.49 4.49 0 0 1 12 2.25c1.357 0 2.573.6 3.397 1.549a4.49 4.49 0 0 1 3.498 1.307 4.491 4.491 0 0 1 1.307 3.497A4.49 4.49 0 0 1 21.75 12a4.49 4.49 0 0 1-1.549 3.397 4.491 4.491 0 0 1-1.307 3.497 4.491 4.491 0 0 1-3.497 1.307A4.49 4.49 0 0 1 12 21.75a4.49 4.49 0 0 1-3.397-1.549 4.49 4.49 0 0 1-3.498-1.306 4.491 4.491 0 0 1-1.307-3.498A4.49 4.49 0 0 1 2.25 12c0-1.357.6-2.573 1.549-3.397a4.49 4.49 0 0 1 1.307-3.497 4.49 4.49 0 0 1 3.497-1.307Zm7.007 6.387a.75.75 0 1 0-1.22-.872l-3.236 4.53L9.53 12.22a.75.75 0 0 0-1.06 1.06l2.25 2.25a.75.75 0 0 0 1.14-.094l3.75-5.25Z" clip-rule="evenodd" />
                                </svg>
                                </span>
                                <span v-if="book.reading===2"> <!--Lettura in corso-->
                                    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="size-6">
                                        <path fill-rule="evenodd" d="M6.32 2.577a49.255 49.255 0 0 1 11.36 0c1.497.174 2.57 1.46 2.57 2.93V21a.75.75 0 0 1-1.085.67L12 18.089l-7.165 3.583A.75.75 0 0 1 3.75 21V5.507c0-1.47 1.073-2.756 2.57-2.93Z" clip-rule="evenodd" />
                                    </svg>
                                </span>
                                <span v-if="book.reading===3"> <!--Lettura ancora non iniziata-->
                                    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="size-6">
                                        <path fill-rule="evenodd" d="M12 1.5a5.25 5.25 0 0 0-5.25 5.25v3a3 3 0 0 0-3 3v6.75a3 3 0 0 0 3 3h10.5a3 3 0 0 0 3-3v-6.75a3 3 0 0 0-3-3v-3c0-2.9-2.35-5.25-5.25-5.25Zm3.75 8.25v-3a3.75 3.75 0 1 0-7.5 0v3h7.5Z" clip-rule="evenodd" />
                                    </svg>
                                </span>
                            </td>
                            <td>
                                <div class="flex items-center gap-3">
                                    <!-- <div class="avatar">
                                        <div class="mask mask-squircle h-12 w-12">
                                            <img
                                            src="../../public/bleach.jpg"
                                            alt="Avatar Tailwind CSS Component" />
                                        </div>
                                    </div> -->
                                    <div>
                                    <div class="font-bold">{{book.title}}</div>
                                        <div class="text-sm opacity-50">{{book.author}}</div>
                                    </div>
                                </div>
                            </td>
                            <td>
                                {{book.volumes}}
                            </td>
                            <td>
                                {{book.status}}
                            </td>
                            <td>
                                <div class="flex">
                                    <i class="fa-regular fa-heart ms-1" :class="{'fa-solid' : book.rating >=1}"></i>
                                    <i class="fa-regular fa-heart ms-1" :class="{'fa-solid' : book.rating >=2}"></i>
                                    <i class="fa-regular fa-heart ms-1" :class="{'fa-solid' : book.rating >=3}"></i>
                                    <i class="fa-regular fa-heart ms-1" :class="{'fa-solid' : book.rating >=4}"></i>
                                    <i class="fa-regular fa-heart ms-1" :class="{'fa-solid' : book.rating >=5}"></i>
                                </div>
                            </td>
                            <td>
                            <button class="btn btn-ghost btn-xs">Modifica</button>
                            </td>
                        </tr>
                        
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>
</div>
</template>
<style scoped>
@import '@fortawesome/fontawesome-free/css/fontawesome.css';
@import '@fortawesome/fontawesome-free/css/regular.css';
@import '@fortawesome/fontawesome-free/css/solid.css';
@import '@fortawesome/fontawesome-free/css/brands.css';
</style>
