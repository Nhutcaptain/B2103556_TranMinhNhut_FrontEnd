<script>
export default {
    props: {
        contacts: { type: Array, default: [] },
        activeIndex: { type: Number, default: -1 },
    },
    emits: ["update:activeIndex"],
    methods: {
        updateActiveIndex(index) {
            this.$emit("update:activeIndex", index);
        }
    }
};
</script>
<template>
    <ul class="list-group">
        <li class="list-group-item" v-for="(contact, index) in contacts" :key="contact._id"
            :class="{ active: index === activeIndex }" @click="updateActiveIndex(index)">
            {{ contact.name }} 
        </li>
    </ul>
</template>
/*Biến contact là biến có thể thay đổi được */
/* {{contact.name}}  Hiển thị tên của contact */
/*Ở đây biến index là mặc định của vue dùng để đếm trong thẻ li */
<style>
    .list-group{
        max-width: 300px;
        position: relative;
        left: 20%;
        text-align: left;
    }

    .list-group-item:hover{
        background-color: rgb(74, 139, 139);
    }
</style>