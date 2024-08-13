<script setup>

import { reactive, ref } from 'vue'

    const emit = defineEmits(["search-term"]);
    const radioOptions = ref(['general', 'author', 'date']);

    const search = reactive({
        value: '',
        searchBy: 'general'
    })

    const onChange = (e) => {
        e.preventDefault();
    }

    const onSearchClick = () => {
        emit("search-term", search);
    }
</script>

<template>

    <div class="card">
        <p>Search by:</p>

        <div class="flex flex-row">
            <div v-for="choice in radioOptions" :key="choice" class="mr-4 mt-2 mb-2">
                <input
                    :id="choice"
                    v-model.lazy="search.searchBy"
                    :value="choice"
                    type="radio"
                    :name="choice"
                    @change="onChange"
                    class="mr-1"
                    />
                <label :for="choice">{{ choice }}</label>
            </div>
        </div>

        <div>
            <input type="text" v-model="search.value" class="input-text" />
            <button @click="onSearchClick()" class="button-regular">Search</button>
        </div>

    </div>
    
</template>

<style scoped>

.input-text {
    width: 80%;
}

</style>