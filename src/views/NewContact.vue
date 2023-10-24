<template>
    <div class="page">
        <h4>Thêm liên hệ</h4>
        <ContactForm :contact="contact" @submit:contact="createContact" />
        <p>{{ message }}</p>
    </div>
</template>

<script>
    import ContactForm from "@/components/ContactForm.vue";
    import ContactService from "@/services/contact.service";
    export default {
        components: {
            ContactForm,
        },
        data() {
           return {
                contact: {
                    name:"",
                    email:"",
                    address:"",
                    phone:"",
                },
                message: "",
           };
        },

        methods: {
            async createContact(data) {
               try {
                await ContactService.create(this.contact);
                this.message = "Đã tạo mới một liên hệ";
                alert(this.message);
                this.$router.push({name: "contactbook"});
               }
               catch(error) {
                console.log(error);
               }
            },

            consoleMessage() {
                console.log(this,this.contact);
            }
        }
    }
</script>