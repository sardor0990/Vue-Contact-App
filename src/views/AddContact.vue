<template>
    <div class="container mt-3">
        <div class="row">
            <div class="col">
                <p class="h3 text-secondary fw-bold">Add Conact</p>
            </div>
        </div>
    </div>
 
    <!-- {{contact}} -->

    <div class="container mt-3">
        <div class="row">
            <div class="col-md-4">
                <form @submit.prevent="submitCreate">
                    <input required v-model="contact.name" type="text" class="form-control mb-2" placeholder="Name">
                    <input required v-model="contact.photo" type="text" class="form-control mb-2" placeholder="Photo URL">
                    <input required v-model="contact.email" type="email" class="form-control mb-2" placeholder="Email">
                    <input required v-model="contact.email2" type="email" class="form-control mb-2" placeholder="Email 2">
                    <input required v-model="contact.phone" type="number" class="form-control mb-2" placeholder="Phone">
                    <input required v-model="contact.phone2" type="number" class="form-control mb-2" placeholder="Phone 2">
                    <input required type="submit" class="btn btn-primary" value="Добавить">
                </form>
            </div>

            <div class="col-md-4 mx-5">
                <img :src="contact.photo" alt="" class="contact-img">
            </div>
        </div>
    </div>
</template>

<script>
import ContactService from '../services/ContactService.js'
export default {
    name: "AddContact",
    data() {
        return {
            contact: {
                name: '',
                email: '',
                email2: '',
                phone: '',
                phone2: '',
                photo: ''
            }
        }
    },
    methods:{
       async submitCreate(){
            try{
                let response = await ContactService.createContact(this.contact);
                if(response){
                    return this.$router.push('/');
                }else{
                    return this.$router.push('/contacts/add');
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