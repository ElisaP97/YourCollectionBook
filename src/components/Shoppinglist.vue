<script setup>
    //componenti importati 

    //librerie e funzioni importate
    import axios from 'axios';
    import { onMounted } from 'vue';
    import { ref } from 'vue';
    import { computed } from 'vue';


    //logica

    const add= ref(1);

    //ci salviamo il percorso al json in una variabile per evitare di riscriverlo sempre
    const urlProducts= "http://localhost:3000/products"
    const products= ref([]); // mi salvo reattivamente la lista dei prodotti in una variabile che inizializzo come un array vuoto

    // mounted riceve come parametro una callback e una volta che il componente verrà montato eseguirà il codice all'interno di essa 
    onMounted(() => { 
     //andiamo a richiedere alla libreria axios una richiesta Http di tipo get(perchè vogliamo visualizzare i prodotti) al percorso indicato, 
     //per ricevere una risposta utilizziamo il metodo then (come per la fetch), il parametro del metodo sarà la risposta
        axios.get(urlProducts).then(resp => {
            console.log(resp); // controlliamo se la chiamata viene effettuata e analizziamo cosa devo puntare per prendere i dati (resp.data)
            products.value = resp.data; // accedo alla proprietà value della funzione ref per salvare i dati nel valore della variabile 
        })
    })

    const nameproduct = ref('');
    const priceproduct = ref('');

    function addProduct(){
        let name= nameproduct._value;
        let price = priceproduct._value;

        const newProduct = {
            name : name,
            price : price,
            completed : false, 
        }

        axios.post(urlProducts, newProduct).then(resp=>{
            // nella risposta andiamo ad aggiornare la lista dei prodotti
            //la richiamiamo e andiamo a riassegnare il valore
            // andiamo ad inserire tutti i prodotti precedenti (... -> splat-operator) che davanti a un array lo va a rompere, estrapola tutti gli elementi di questo array
            // e dopo la virgola aggiungo il nuovo prodotto
            products.value = [...products.value, resp.data]; 
        })

        // reset campi input
        nameproduct._value='';
        priceproduct._value='';
    }

    function toggleCompleted(product){
        //patch è il metodo http va a modificare una singola proprietà dell'oggetto, è più selettivo di put  
        //il primo parametro è il percorso del prodotto
        //si usa i backtick per concatenare le stringhe andando a richiamare le variabili, si concatena l'url e l'id dell'oggetto per arrivare al suo singolo percorso
        //secondo parametro il prodotto stesso da modificare
        axios.patch(`${urlProducts}/${product.id}` , product).then(resp=>{
            console.log(resp);
        })

    }

    function deleteProduct(id){
        axios.delete(`${urlProducts}/${id}`).then(resp=>{
        //come per l'aggiunta anche per l'eliminazione andiamo ad aggiornare la lista dei prodotti
        //per farlo filtriamo l'arrey andando a dire di prendere tutti i prodotti con id diverso da quello eliminato
            products.value = products.value.filter(p => p.id != id); 
        });
    }

    // prodotti comprati
    //tiene traccia delle sue dipendenze reattive, terrà traccia dei cambiamenti in automatico
    //come parametro accetta una callback quindi bisogna indicargli di che cosa vuole tenere traccia, in questo caso la lista dei prodotti completati
    let completedProducts= computed(()=>products.value.filter(p => p.completed));

    //prezzo totale dei manga che voglio comprare 
    let TotPrice = computed(()=>products.value.map(p => p.price).reduce((acc,p) => acc + p,0));
    //prezzo totale speso dei manga comprati 
    let PriceTotCompletedProducts= computed(()=>products.value.filter(p => p.completed).map(p => p.price).reduce((acc,p) => acc + p,0));
    

</script>

<template>
    <!-- div contenitore della pagina collezione -->
    <div class="flex md:container md:mx-auto h-screen justify-center">
        <!-- div contenitore con immagine manga -->
        <div class="bg-accent-content h-3/4 sm:4/5 md:w-4/5" style="background-image: url(../../public/manga2.jpg)">
            <!-- div contenitore barra di ricerca e lista -->
            <div class="bg-primary-content h-full w-full bg-opacity-80 flex justify-center flex-wrap content-start gap-2">                
                <button @click="add=2" v-if="add===1" class="btn btn-xs lg:btn-lg lg:m-4 btn-outline btn-accent bg-accent-content mt-5 ">
                    Aggiungi Prodotto
                    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="size-6">
                        <path fill-rule="evenodd" d="M12 2.25c-5.385 0-9.75 4.365-9.75 9.75s4.365 9.75 9.75 9.75 9.75-4.365 9.75-9.75S17.385 2.25 12 2.25ZM12.75 9a.75.75 0 0 0-1.5 0v2.25H9a.75.75 0 0 0 0 1.5h2.25V15a.75.75 0 0 0 1.5 0v-2.25H15a.75.75 0 0 0 0-1.5h-2.25V9Z" clip-rule="evenodd" />
                    </svg>
                </button>
                <div v-if="add===2" class="border border-accent rounded-lg bg-accent-content p-2 mt-5 flex flex-wrap justify-center align-center gap-2" >
                        <label class="input input-bordered input-accent text-accent flex items-center gap-2 ">
                            Nome
                            <input  v-model.trim="nameproduct" type="text" class="grow" placeholder="Inserisci il titolo" />
                        </label>
                        <label class="input input-bordered input-accent text-accent flex items-center gap-2">
                            Prezzo
                            <input v-model.number="priceproduct" type="text" class="grow" placeholder="Inserisci il prezzo" />
                        </label>
                        <button @click="addProduct()" class="text-accent border border-1 border-accent rounded my-2 px-4 md:p-0 md:my-0 md:border-none "> <!--keyup evento del Dom quando rilascio il bottone specificato-->
                            Enter
                        </button>
                </div>
                <div class="bg-accent-content border border-accent border-4 w-4/5 h-3/4 p-3 overflow-auto">
                    <h1 class="text-center text-2xl sm:text-4xl sm:text-5xl mb-2">Shopping List</h1>
                    <div class="grid grid-rows-6 grid-flow-col gap-4 mt-5 pb-3 h-3/4">
                        <!-- Lista nuove uscite -->
                        <div class="row-span-5 col-span-3 overflow-auto ">
                            <div class="h-full">
                                    <table class="table">
                                        <!-- head -->
                                        <thead>
                                            <tr>
                                                <th></th>
                                                <th>Nome</th>
                                                <th>Prezzo</th>
                                                <th>Check</th>
                                            </tr>
                                        </thead>
                                        <tbody v-for="product in products" :key="product.id" class="divide-y"> <!-- ciclo tutti gli elementi del json distinguendoli con l'id attraverso l'attributo key-->
                                        <!-- singolo prodotto -->
                                        <tr>
                                            <th>
                                                <button @click="deleteProduct(product.id)" class="btn btn-xs sm:btn-sm btn-circle border-0 btn-outline btn-accent bg-secondary-content" >
                                                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="size-6">
                                                        <path stroke-linecap="round" stroke-linejoin="round" d="m14.74 9-.346 9m-4.788 0L9.26 9m9.968-3.21c.342.052.682.107 1.022.166m-1.022-.165L18.16 19.673a2.25 2.25 0 0 1-2.244 2.077H8.084a2.25 2.25 0 0 1-2.244-2.077L4.772 5.79m14.456 0a48.108 48.108 0 0 0-3.478-.397m-12 .562c.34-.059.68-.114 1.022-.165m0 0a48.11 48.11 0 0 1 3.478-.397m7.5 0v-.916c0-1.18-.91-2.164-2.09-2.201a51.964 51.964 0 0 0-3.32 0c-1.18.037-2.09 1.022-2.09 2.201v.916m7.5 0a48.667 48.667 0 0 0-7.5 0" />
                                                    </svg>
                                                </button>
                                            </th>
                                            <th :class="{'line-through text-accent': product.completed}">{{product.name }}</th>
                                            <th :class="{'text-error': product.completed}">{{product.price}}€</th>
                                            <th><input type="checkbox" class="checkbox checkbox-accent" @change="toggleCompleted(product)" v-model="product.completed" :checked="product.completed" /></th>
                                        </tr>
                                        </tbody>
                                    </table>
                            </div>
                        </div>
                        <!-- Prezzo totale che spendo -->
                        <div class="col-span-3 mt-3 text-center">
                            <p class="text-xs sm:text-sm md:text-lg">Totale Prezzo da spendere: {{ TotPrice }} € </p>
                            <p class="text-xs sm:text-sm md:text-lg">Totale Speso: {{ PriceTotCompletedProducts }} € </p>
                        </div>
                        <!-- da inserire in libreria 
                        <div class="row-span-6 col-span-2 border text-center p-2 overflow-x-auto h-full">
                            <h2 class="text-xl">Da aggiungere in collezione</h2>
                            <div class="mt-2" v-for="product in completedProducts" :key="product.id">
                                <p class="text-center">{{product.name}}</p>
                            </div>
                        </div>-->
                    </div>
                </div>
            </div>
        </div>
    </div>
    </template>


<style scoped>
</style>
