<script>
export default {
    props: {
        modelvalue: {type: String, default:""},
    },
    emits: ["submit", "update:modelValue"],
    methods: {
        updateModelValue(e) {
            this.$emit("update:modelValue", e);
        },
        submit() {
            this.updateModelValue(this.$refs.inputRef.value); //Lấy value trong input field thông qua inputRef
            this.$emit("submit");
        },
    },
};
</script>

<template> 
    <div class="input-group">
        <input type="text"
                class="form-control"
                placeholder="Nhap thong tin can tim"
                :value="modelvalue"
                id="TestCode" 
                @keyup.enter="submit"
                ref="inputRef" 
        />
        <!--ref = "inputRef" lấy dữ liệu trong input field -->
        <div class="input-group-append">
            <button 
                class="btn btn-outline-secondary"
                type="button"
                @click="submit">
                <i class="fa-brands fa-searchengin"></i>Tim kiem
            </button>
        </div>
    </div>    
</template>

<style>
    .input-group{
        width: 500px;
        position: absolute;
        margin-bottom: 20px;
        padding-bottom: 100px;
        left: 0%;
    }
</style>