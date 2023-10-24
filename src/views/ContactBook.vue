<template>
    
    <div class="pageview">
        <div class="search-form">
            <InputSearch v-model="searchText"/>
            
        </div>
       <div class="main-content row">
        <div class="col-sm-6 align-items-center">
            <h4>
                Danh bạ
                <i class="fas fa-address-book"></i>
            </h4>
            <ContactList v-if="filteredContactsCount > 0" :contacts="filteredContacts" v-model:activeIndex="activeIndex"/>
            <p v-else>Không có liên hệ nào.</p>
            <div class="mt-3 row align-items-center" id="button-list">
                <button class="btn btn-sm btn-primary" @click="refreshList()">
                    <i class="	far fa-file"></i> Làm mới
                </button>
                <button class="btn btn-sm btn-success" @click="goToAddContact">
                    <i class="fas fa-plus"></i> Thêm mới
                </button>
                <button class="btn btn-sm btn-danger" @click="removeAllContacts">
                    <i class="	fas fa-trash-alt"></i> Xóa tất cả
                </button>
            </div>
        </div>
        <div class="col-sm-6 ">
            <div v-if="activeContact" class="contactCard">
                <h4>
                    Chi tiết Liên hệ
                    <i class="fas fa-address-card"></i>
                </h4>
                <ContactCard :contact="activeContact"></ContactCard>
                <router-link
                    :to="{
                        name:'contact.edit',
                        params: {id: activeContact._id},
                    }"
                >
                <span class="mt-2 badge badge-warning edit-button">
                    <i class="fas fa-edit"></i> Hiệu chỉnh
                </span>
                </router-link>

                
            </div>
        </div>
       </div>
    </div>
</template>
<script>
import ContactCard from "@/components/ContactCard.vue";
import InputSearch from "@/components/InputSearch.vue";
import ContactList from "@/components/ContactList.vue";
import ContactService from "@/services/contact.service";
export default {
    components: {
        ContactCard,
        InputSearch,
        ContactList,
    },
    data() {
        return {
            contacts: [],
            activeIndex: -1,
            searchText: "",
        };
    },
    watch: {
        // Giám sát các thay đổi của biến searchText.
        // Bỏ chọn phần tử đang được chọn trong danh sách.
        searchText() {
            this.activeIndex = -1;
        },
    },
    computed: {
        // Chuyển các đối tượng contact thành chuỗi để tiện cho tìm kiếm.
        contactStrings() {
            return this.contacts.map((contact) => {
                const { name, email, address, phone } = contact;
                return [name, email, address, phone].join("");
            });
        },
        // Trả về các contact có chứa thông tin cần tìm kiếm.
        filteredContacts() {
            if (!this.searchText) {
                return this.contacts;
                
            }
            return this.contacts.filter((_contact, index) =>
                this.contactStrings[index].includes(this.searchText)
            );
        },
        activeContact() {
            if (this.activeIndex < 0) return null;
            return this.filteredContacts[this.activeIndex];
            
        },
        filteredContactsCount() {
            console.log(this.filteredContacts.length)
            return this.filteredContacts.length;
        },
    },
    methods: {
        async retrieveContacts() {
            try {
                this.contacts = await ContactService.getAll(); //dùng để gán data trong server cho mảng contacts
            } catch (error) {
                console.log(error);
            }
        },
        refreshList() {
            this.searchText = "";
            this.retrieveContacts();
            this.activeIndex = -1;
        },
        async removeAllContacts() {
            if (confirm("Bạn muốn xóa tất cả Liên hệ?")) {
                try {
                    await ContactService.deleteAll();
                    this.refreshList();
                } catch (error) {
                    console.log(error);
                }
            }
        },
        goToAddContact() {
            this.$router.push({ name: "contact.add" });
        },
    },
    mounted() {
        this.refreshList();
    },
};
</script>
<style scoped>
.page {
text-align: left;
max-width: 750px;
}  
.edit-button{
    position:absolute;
    top: 50%;
    text-align: left;
}

#button-list{
   position: relative;
   left: 20%;
}

#button-list button{
    margin-left: 15px;
}
</style>