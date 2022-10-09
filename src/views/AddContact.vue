<template>
    <div class="container mt-3">
        <div class="row">
            <div class="col">
                <p class="h3 text-success fw-bold">Add Contact</p>
                <p class ="fst-italic">Lorem ipsum dolor sit amet consectetur adipisicing elit. Voluptas placeat voluptates aliquam debitis maxime maiores sint tenetur, nostrum doloribus recusandae ipsam dolorum aliquid architecto, deleniti nisi labore nesciunt totam? Neque.</p>
            </div>
        </div>
    </div>
    <div class="container mt-3">
        <div class="row">
            <div class="col-md-4">
                <form @submit.prevent="submitCreate()">
                    <div class="mb-2">
                        <input v-model="contact.name" type="text" class="form-control" placeholder="Name">
                    </div>
                    <div class="mb-2">
                        <input type="text" v-model="contact.photo" class="form-control" placeholder="PhotoURL">
                    </div>
                    <div class="mb-2">
                        <input type="email" v-model="contact.email" class="form-control" placeholder="Email">
                    </div>
                    <div class="mb-2">
                        <input type="number" v-model="contact.mobile" class="form-control" placeholder="Mobile">
                    </div>
                    <div class="mb-2">
                        <input type="text" v-model="contact.company" class="form-control" placeholder="Company">
                    </div>
                    <div class="mb-2">
                        <input type="text" v-model="contact.title" class="form-control" placeholder="Title">
                    </div>
                    <div class="mb-2">
                        <select class="form-control" v-model="contact.groupId"  v-if="groups.length >0">
                            <option :value="group.id" v-for="group of groups" :key="group.id">{{group.name}}</option>
                        </select>
                    </div>
                    <div class="mb-2">
                        <input type="submit" value="Create" class="btn btn-success">
                    </div>
                </form>
                
            </div>
            <div class="col-md-4">
                <img :src="contact.photo" class="contact-img-big">
            </div>
        </div>
    </div>
</template>
<script>
import { ContactService } from '@/services/ContactService'
export default {
    data:function(){
        return{
            contact:{
                name:"",
                photo:"",
                email:"",
                mobile:"",
                company:"",
                title:"",
                groupId:""
            },
            groups:[]
        }
    },
    created:async function(){
        try {
            let response =await  ContactService.getAllGroups();
            this.groups =response.data
        } catch (error) {
        console.log(error);            
        }
    },
    methods:{
        submitCreate:async function(){
            try {
                let response = await  ContactService.createContact(this.contact);
                if(response){
                    return this.$router.push("/");

                }
                else{
                    return this.$router.push("/contacts/add");
                }
            } catch (error) {
                console.log(error)
            }
        }
    }

}
</script>
<style >
    
</style>