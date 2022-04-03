<template>
    <div class="container my-3">
        <div class="row">
            <div class="col">
                <p class="h3 text-secondary fw-bold">View Contact</p>
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


    <div class="container" v-if="!loading && isDone()">
        <div class="row d-flex justify-content-center align-items-center">
            <div class="col-md-4">
                <img :src="contact.photo" alt="" class="contact-img-big">
            </div>

            <div class="col-md-6">
                <ul class="list-group">
                    <li class="list-group-item">Name: <span class="fw-bold">{{ contact.name }}</span> </li>
                    <li class="list-group-item">Email: <span class="fw-bold">{{ contact.email }}</span> </li>
                    <li class="list-group-item">Email 2: <span class="fw-bold">{{ contact.email2 }}</span> </li>
                    <li class="list-group-item">Phone: <span class="fw-bold">{{ contact.phone }}</span> </li>
                    <li class="list-group-item">Phone 2: <span class="fw-bold">{{ contact.phone2 }}</span> </li>
                 </ul>
            </div>
        </div>

        <div class="row mt-5">
            <div class="col">
                <router-link to="/" class="btn btn-primary"><i class="fa fa-arrow-alt-circle-left"></i> Назад</router-link>
            </div>
        </div>
    </div>
    <!-- {{contact}} -->

</template>

<script>
import ContactService from '../services/ContactService.js'
import Spinner from '../components/Spinner.vue';
export default {
    name: "ViewContact",
    components: {Spinner},
    data(){
        return {
            contactId: this.$route.params.contactId,
            loading: false,
            contact: {},
            errorMessage: null
        }
    },
   async created(){
       try{
           this.loading = true;
           let response = await ContactService.getContact(this.contactId);
           this.contact = response.data;
           this.loading = false;
       }catch(error){
           this.errorMessage = error;
           this.loading = false;
       }
   },
   methods:{
       isDone(){
           return Object.keys(this.contact).length > 0
       }
   }
}
</script>

<style scoped>

</style>