<template>
    <div class="product-item">
        <h3>{{ product.name }}</h3>
        <p>Продавец: {{ product.seller }}</p>
        <p>Интеграция: {{ product.integration }}</p>
        <div class="actions">
            <button class="btn btn-danger" @click="$emit('delete', product.id)">Удалить</button>
        </div>
    </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
    props: {
        product: {
            type: Object,
            required: true,
        },
    },
});
</script>

<style lang="scss" scoped>
.product-item {
    border: 1px solid #ddd;
    padding: 15px;
    margin-bottom: 10px;
    margin-right: 10px;
    border-radius: 8px;
    background-color: #fff;

    h3 {
        margin-top: 0;
    }

    .actions {
        display: flex;
        justify-content: end;
        gap: 10px;

        .btn {
            padding: 8px 12px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            font-size: 14px;
        }

        .btn-secondary {
            background-color: #6c757d;
            color: #fff;

            &:hover {
                background-color: #5a6268;
            }
        }

        .btn-danger {
            background-color: #dc3545;
            color: #fff;

            &:hover {
                background-color: #c82333;
            }
        }
    }
}
.product-item:last-child {
    margin-right: 0px;
}
</style>