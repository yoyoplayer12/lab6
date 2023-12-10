<script setup>
    import { ref, onMounted, onBeforeMount } from 'vue';
    
    const props = defineProps({
        options:{
            type: Array,
            required: true
        },
    });

    const selectedoption = ref(null);
    const DropDownVisible = ref(false);

    const emit = defineEmits(['selected'])

    const closeDropDown = () => {
        DropDownVisible.value = false;
    }

    const pickOption = (option) => {
        selectedoption.value = option;
        closeDropDown();
        emit('selected', selectedoption.value);
    }



</script>

<template>
    <div class="dropdown">
        <div class="dropdown-selected" :class="{ 'dropdown-visible': DropDownVisible }" @click="DropDownVisible = !DropDownVisible">
            {{ selectedoption || "Select" }}
        </div>
        <div class="options-wrapper" v-if="DropDownVisible">
            <div class="option" v-for="(option, index) in options" :key="index" @click="pickOption(option)">
                {{ option }}
            </div>
        </div>
    </div>
</template>

<style scoped>
    .dropdown-selected{
        border: 1px whitesmoke solid;
        padding: 5px 25px;
        width: 150px;
        height: 30px;
        border-radius: 10px 10px 10px 10px;
    }
    .dropdown-selected:hover{
        cursor: pointer;
    }
    .option{
        border: 1px whitesmoke solid;
        padding: 5px 25px;
        width: 150px;
    }
    .option:hover{
        background-color: whitesmoke;
        color: black;
        cursor: pointer;
    }
    .option:last-of-type{
        border-radius: 0 0 10px 10px;
    }
    .dropdown-visible {
        border-radius: 10px 10px 0px 0px;
    }
</style>
