<template>
    <div class="container mt-3">
        <div class="row">
            <div class="col">

                <p class="h3 text-secondary fw-bold">
                   Vue Contact App 
                    <router-link to="/contacts/add" class="btn btn-primary btn-sm mx-2"><i class="fa fa-plus-circle"></i> New</router-link> 
                </p>

               
                <form>
                    <div class="row">
                        <div class="col-md-6">
                            <div class="row">
                                <div class="col">
                                    <input v-model="search" type="text" class="form-control" placeholder="Search Name">
                                </div>
                            </div>
                        </div>
                    </div>
                </form>
            </div>
        </div>
    </div>

    <!-- Spinner -->
    <div v-if="loading">
        <div class="container">
            <div class="row d-flex justify-content-center">
                <div class="col-md-4 text-center">
                    <Spinner />
                </div>
            </div>    
        </div>    
    </div> 

    <!-- Error Message -->
    <div v-if="!loading && errorMessage">
        <div class="container mt-3">
            <div class="row d-flex justify-content-center">
                <div class="col-md-4 text-center">
                   <p class="h3 text-danger fw-bold">{{ errorMessage }}</p>
                </div>
            </div>    
        </div>    
    </div> 
   
    <div class="container mt-3" v-if="contacts.length > 0">
        <div class="row">
            <div class="col-md-6" v-for="contact of filteredContact" :key="contact">
                <div class="card my-2 list-group-item-primary shadow-lg">
                    <div class="card-body">
                        <div class="row align-items-center">
                            <div class="col-sm-4">
                                <img :src="contact.photo" alt="" class="contact-img">
                            </div>
                            <div class="col-sm-7">
                                <ul class="list-group">
                                    <li class="list-group-item">Name: <span class="fw-bold">{{ contact.name }}</span> </li>
                                    <li class="list-group-item">Email: <span class="fw-bold">{{ contact.email }}</span> </li>
                                    <li class="list-group-item">Mobile: <span class="fw-bold">{{ contact.phone }}</span> </li>
                                </ul>
                            </div>
                            <div class="col-sm-1 d-flex flex-column align-items-center">
                                
                                <router-link :to="`/contacts/view/${contact.id}`" class="btn btn-primary my-1">
                                    <i class="fa fa-eye"></i>
                                </router-link>

                                <router-link :to="`/contacts/edit/${contact.id}`" class="btn btn-primary my-1">
                                    <i class="fa fa-pen"></i>
                                </router-link>

                                <button class="btn btn-danger my-1" @click="clickDeletePerson(contact.id)">
                                    <i class="fa fa-trash"></i>
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import ContactService from '../services/ContactService.js'
import Spinner from '../components/Spinner.vue';
export default {
    name: "ContactManager",
    components: {Spinner},
    data(){
        return {
            loading: false,
            contacts: [],
            errorMessage: null,
            search: ''
        }
    },

   async created(){
        try{
            this.loading = true;
            let response = await ContactService.getAllContacts();
            this.contacts = response.data;
            this.loading = false;
            console.log(this.contacts)
        }catch(error){
            this.errorMessage = error;
            this.loading = false;
        }
    },

    methods: {
       async clickDeletePerson(contactId){
            try{
                this.loading = true;
                let response = await ContactService.deleteContact(contactId);
                if(response){
                    let response = await ContactService.getAllContacts();
                    this.contacts = response.data;
                    this.loading = false;
                }
            }catch(error){
                this.errorMessage = error;
                this.loading = false;
            }
        }
    },

    computed: {
        filteredContact(){
            return this.contacts.filter((contact) =>{
                return contact.name.match(this.search)
            })
        }
    }
}
</script>

<style scoped>

</style>