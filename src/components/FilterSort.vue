<template>
    <div class="filter-sort">
        <div class="form-group">
            <label for="integration">Фильтр по интеграции:</label>
            <select id="integration" v-model="selectedIntegration" @change="filterProducts">
                <option value="">Все</option>
                <option value="reels">Reels</option>
                <option value="stories">Stories</option>
            </select>
        </div>
        <div class="form-button">
            <button @click="sortProducts('asc')">Сортировать A-Z</button>
            <button @click="sortProducts('desc')">Сортировать Z-A</button>
        </div>
    </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';

export default defineComponent({
    emits: ['filter', 'sort'],
    setup(_, { emit }) {
        const selectedIntegration = ref('');

        const filterProducts = () => {
            emit('filter', selectedIntegration.value);
        };

        const sortProducts = (order: 'asc' | 'desc') => {
            emit('sort', order);
        };

        return {
            selectedIntegration,
            filterProducts,
            sortProducts,
        };
    },
});
</script>

<style lang="scss" scoped>
.filter-sort {
    display: flex;
    align-items: center;
    justify-content: space-around;
    max-width: 700px;
    margin: 20px auto;

    .form-group {
        display: flex;
        align-items: center;

        label {
            display: block;
            margin-right: 5px;
        }

        select {
            padding: 8px;
            border-radius: 4px;
            border: 1px solid #ccc;
        }
    }
    .form-button {
        display: flex;
        margin-left: 10px;
    }

    button {
        padding: 8px 12px;
        margin-right: 10px;
        background-color: #007bff;
        color: #fff;
        border: none;
        border-radius: 4px;
        cursor: pointer;

        &:hover {
            background-color: #0056b3;
        }
    }
}
</style>