<template>
    <div class="container mt-3">
        <div class="row">
            <div class="col">
                <p class="h3 fw-bold text-success">Contact Manager
                    <router-link to="/contacts/add" class ="btn btn-success btn-sm"><i class="fa fa-plus-circle"></i>New</router-link>
                </p>
                <p class="fst-italic">Lorem ipsum dolor sit, amet consectetur adipisicing elit. Sed quaerat ea, minus fugiat non vel veniam, cum enim vitae commodi, iusto aut quasi. Exercitationem, dolorum beatae earum numquam quis quos.</p>
                
            </div>
        </div>
    </div>


    <div v-if="loading">
        <div class="conrainer">
            <div class="row">
                <div class="col">
                    <SpinnerComponent></SpinnerComponent>
                </div>
            </div>
        </div>
    </div>

    <div v-if="!loading && errorMessage" >
        <div class="container">
            <div class="row">
                <div class="col">
                    <p class="h3 text-danger fw-bold">{{errorMessage}}</p>
                </div>
            </div>
        </div>
    </div>


    <div class="container mt-3" v-if="contacts.length > 0">
        <div class="row">
            <div class="col-md-6" v-for="contact of contacts" :key="contact">
                <div class="card my-2 list-group-item-success shadow-lg" >
                    <div class="card-body">
                        <div class="row align-items-center">
                            <div class="col-sm-4">
                                <img :src="contact.photo" class="contact-img">
                            </div>
                            <div class="col-sm-7">
                                <ul class="list-group">
                                    <li class="list-group-item">
                                        Name:<span class="fw-bold">{{contact.name}}</span>
                                    </li>
                                    <li class="list-group-item">
                                        Email:<span class="fw-bold">{{contact.email}}</span>
                                    </li>
                                    <li class="list-group-item">
                                        Mobile:<span class="fw-bold">{{contact.mobile}}</span>
                                    </li>
                                </ul>
                            </div>
                            <div class="col-sm-1 d-flex flex-column justify-content-center align-items-center">
                                <router-link :to ="`/contacts/view/${contact.id}`" class="btn btn-warning my-1"><i class="fa fa-eye"></i></router-link>
                                <router-link :to ="`/contacts/edit/${contact.id}`" class="btn btn-primary my-1"><i class="fa fa-pen"></i></router-link>
                              <button class="btn btn-danger" @click="clickDeleteContact(contact.id)">
                                <i class="fa fa-trash my-1"></i>
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
import { ContactService } from '@/services/ContactService';
import SpinnerComponent from '@/components/SpinnerComponent.vue';
export default {
    data: function () {
        return {

            loading: false,
            contacts: [], 
            errorMessage: null
        };
    },
    created: async function () {
        try {
            this.loading = true;
            let response = await ContactService.getAllContacts();
            this.contacts = response.data;
            this.loading = false;
        }
        catch (error) {
            this.errorMessage = error;
            this.loading = false;
        }
    },
    methods: {
        clickDeleteContact:async function (contactId) {
            try {
                this.loading=true;
                let response= await ContactService.deleteContact(contactId);
                if(response){
                    let response = await  ContactService.getAllContacts();
                    this.contacts = response.data;
                    this.loading=false
                }
            } catch (error) {
            this.errorMessage = error;
            this.loading = false;
            }
        }
    },
    components: { SpinnerComponent }
}
</script>
<style>
    
</style>