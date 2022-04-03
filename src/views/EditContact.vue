<template>
    <div class="container mt-3">
        <div class="row">
            <div class="col">
                <p class="h3 text-secondary fw-bold">Edit Contact</p>
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

    <div class="container mt-3">
        <div class="row">
            <div class="col-md-4">
                <form @submit.prevent="updateSubmit()">
                    <input v-model="contact.name" type="text" class="form-control mb-2" placeholder="Name">
                    <input v-model="contact.photo" type="text" class="form-control mb-2" placeholder="Photo URL">
                    <input v-model="contact.email" type="email" class="form-control mb-2" placeholder="Email">
                    <input v-model="contact.email2" type="email" class="form-control mb-2" placeholder="Email 2">
                    <input v-model="contact.phone" type="number" class="form-control mb-2" placeholder="Phone">
                    <input v-model="contact.phone2" type="text" class="form-control mb-2" placeholder="Phone 2">
                    <input type="submit" class="btn btn-primary" value="Обновить">
                </form>
            </div>

            <div class="col-md-4 mx-5">
                <img :src="contact.photo" alt="" class="contact-img">
            </div>
        </div>
    </div>
    <!-- {{contact}} -->
</template>


<script>
import ContactService from '../services/ContactService.js';
import Spinner from '../components/Spinner.vue';
export default {
    name: "EditContact",
    components: {Spinner},
    data(){
        return{
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
   methods: {
      async updateSubmit(){
           try{
                let response = await ContactService.updateContact(this.contact, this.contactId);
                if(response){
                    return this.$router.push('/');
                }else{
                    return this.$router.push(`/contacts/edit/${this.contactId}`);
                }
            }catch(error){
                console.log(error);
            }
       }
   }
}
</script>

<style scoped>

</style>