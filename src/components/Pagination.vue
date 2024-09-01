<template>
    <div class="pagination">
        <button @click="prevPage" :disabled="currentPage === 1">Предыдущая</button>
        <span>{{ currentPage }} / {{ totalPages }}</span>
        <button @click="nextPage" :disabled="currentPage === totalPages">Следующая</button>
    </div>
</template>

<script lang="ts">
import { defineComponent,  } from 'vue';

export default defineComponent({
    props: {
        total: {
            type: Number,
            required: true,
        },
        currentPage: {
            type: Number,
            required: true,
        },
    },
    emits: ['change'],
    computed: {
        totalPages() {
            return Math.ceil(this.total / 20); // 20 товаров на страницу
        },
    },
    methods: {
        prevPage() {
            if (this.currentPage > 1) {
                this.$emit('change', this.currentPage - 1);
            }
        },
        nextPage() {
            if (this.currentPage < this.totalPages) {
                this.$emit('change', this.currentPage + 1);
            }
        },
    },
});
</script>

<style lang="scss" scoped>
.pagination {
    display: flex;
    justify-content: space-between;
    align-items: center;
    max-width: 500px;
    margin: 20px auto;

    button {
        padding: 10px;
        background-color: #007bff;
        color: #fff;
        border: none;
        border-radius: 4px;
        cursor: pointer;

        &:disabled {
            background-color: #ccc;
            cursor: not-allowed;
        }

        &:hover:not(:disabled) {
            background-color: #0056b3;
        }
    }
}
</style>