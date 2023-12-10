<script setup>
    import DropDown from './dropdown/dropdown.vue'
    import { ref } from 'vue'

    const options = ref(["Jeff", "Mary", "Jozeph", "Frank"]);
    const selectedOption = ref(null);

    const handleSelection = (option) => {
        selectedOption.value = option;
    }
</script>

<template>
    <div class="dropdown-grid">
        <DropDown 
        :options="options"
        @selected="handleSelection"
        />
        <input type="number" id="score" name="score" placeholder="0">
        <button>Update Score for {{ selectedOption }}</button>
    </div>
</template>

<style scoped>
    .dropdown-grid{
        display: grid;
        grid-template-columns: 200px 200px 400px;
        grid-template-rows: 45px;
        grid-gap: 10px;
    }
    input{
        border-radius: 10px;
        border: none;
    }
    button{
        border-radius: 10px;
    }
</style>
