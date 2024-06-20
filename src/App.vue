<script >
import axios from 'axios';

export default {
    name: 'App',
    data(){
        return{
            base_api_url: 'http://127.0.0.1:8000',
            photos_endpoint: '/api/photos',
            categories_endpoint: '/api/categories',
            categories: [],
            photos: '',
            search_text: '',
            filter_category: '',
        }
    },
    methods: {
        filterCategories(){
            const urlCategory= this.base_api_url + this.categories_endpoint + `?filter=${this.filter_category}`
            axios
            .get(urlCategory)
            .then(resp => {
                this.categories =resp.data.results
                console.log(this.filterCategories)
            })
            .catch((error) =>{
        console.error("Errore nella richiesta:", error);
      });

        },
        search(){
            const url= this.base_api_url + this.photos_endpoint + `?search=${this.search_text}`
            console.log(url);
            this.callApi(url);
        },

        nextPage(url){
            console.log(url);
            this.callApi(url);
        },
        callApi(url){
            axios
        .get(url)
        .then(resp => {
            console.log(resp);
            this.photos = resp.data.results
            
        })
        .catch((error) => {
        console.error("Errore nella richiesta:", error);
      });
        }
    },
    mounted() {
        const url= this.base_api_url + this.photos_endpoint
        this.callApi(url)
    }
}
</script>

<template>
    <!--HEADER CON NAV E SEARCH-->
    <nav
        class="navbar navbar-expand-sm navbar-light bg-light "
    > 
        <div class="container d-flex justify-content-between ">
            <img width="200" src="./assets/img/logo.jpg" alt="">
            
                <!--SEARCH-->
                <form class="d-flex my-2 my-lg-0" @submit.prevent="search()">
                    
                    <input
                        class="form-control-xs me-sm-2"
                        type="search"
                        name="search"
                        id="search"
                        placeholder="Search"
                        v-model="search_text"
                    />
                    <button
                        class="btn btn-secondary my-2 my-sm-0"
                        type="submit"
                    >
                        Search
                    </button>
                
                </form>
                <!--FILTER CATEGORY

                <div class="mb-3">
                    <select v-model="this.filter_category" multiple @change="filterCategories()" class="form-select form-select-sm ms-3">
                        <option  selected>Select one</option>
                        <option v-for="category in this.categories" :value="category.id">{{category.name}}</option>
                    </select>
                </div>
                -->
            
        </div>
    </nav>
    
    <!--JUMBOTRON CON PULSANTE TUTTE FOTO-->
<div class="p-5 mb-5 bg-light rounded-3 custom-jumbotron">
    <div class="container-fluid py-1">
        <h1 class="display-5 fw-bold">Photographer Pino Pallino</h1>
        <div class="description_profile">
            <h3 class="">Catturare Momenti, Raccontare Storie</h3>
            <p>
                Benvenuti nel mondo di Pinco Pallino, dove ogni scatto è una finestra aperta su emozioni, storie e bellezze del mondo. Con un occhio attento ai dettagli e una passione insaziabile per l'arte visiva, Pinco Pallino trasforma attimi fugaci in ricordi duraturi.
                
                Chi Sono
                Sono Pinco Pallino, un fotografo professionista con oltre 35 anni di esperienza nel settore. La mia avventura nel mondo della fotografia è iniziata 2009 e da allora ho avuto l'opportunità di lavorare con clienti straordinari, esplorare luoghi incredibili e affinare le mie competenze in vari generi fotografici.
                
                Specializzazioni
                Fotografia di Matrimonio: Catturo l'amore e la gioia del vostro giorno speciale, creando immagini che raccontano la vostra storia d'amore in modo autentico e romantico.
                Ritratti: Mi concentro sulla personalità e l'essenza di ogni individuo, creando ritratti che riflettono la loro vera natura.
                Fotografia di Paesaggio: La mia passione per la natura mi porta a catturare scenari mozzafiato, immortalando la bellezza del mondo che ci circonda.
                Fotografia di Eventi: Documentare eventi speciali, dalle feste private agli eventi aziendali, con un approccio discreto e professionale.
                Fotografia Commerciale: Collaboro con aziende per creare immagini accattivanti che rappresentano al meglio i loro prodotti e servizi.
                Filosofia
                Credo che ogni foto racconti una storia e il mio obiettivo è quello di catturare quelle storie nel modo più autentico possibile. Lavoro con dedizione e creatività, cercando sempre nuovi modi per esprimere la mia visione artistica e soddisfare le esigenze dei miei clienti.
            </p>

        </div>

    </div>
</div>


    <!--TUTTE LE FOTO-->
    <section class="photos" v-if="photos">
    <div class="container">
        <div class="row row-cols-1 row-cols-sm-2 row-cols-md-3 g-4">
            <div class="col" v-for="photo in photos.data">
                <div class="card" >
                    <div v-if="photo.cover_image">
                        <img   class="card-img-top" :src="photo.cover_image.startsWith('https://') ? photo.cover_image : base_api_url + '/storage/' + photo.cover_image" :alt="photo.title">
                    </div>
                    <div v-else>
                        <img src="./assets/img/noimage.webp" class="card-img-top" alt="not_photo">
                    </div>
                    
                   
                </div>
            </div>
        </div>
        <!--PAGINATION-->
        <nav aria-label="Page navigation">
        <ul
            class="pagination  mt-4 py-5 "
        >
            <li class="page-item" :class="{'disabled': !link.url, 'active': link.active}" v-for="link in photos.links">
                <button class="page-link btn btn-secondary" :href="link.url" type="button" @click="nextPage(link.url)">
                    <span v-html="link.label"></span>
                </button>
            </li>

        </ul>
    </nav>
    </div>

    
    

</section>





</template>

<style >
.custom-jumbotron {
    background-image: url('./assets/img/jumbotron-photographer.jpg');
    background-size: cover;
    background-position: center;
    color: white;
    overflow: hidden;
    height: 50vh;
    }

    .container-fluid {
    z-index: 2;
    padding: 2rem;
    border-radius: 0.5rem;
    }

    

</style>
