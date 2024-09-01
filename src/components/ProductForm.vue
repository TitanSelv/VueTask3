<template>
    <form @submit.prevent="submitForm" class="product-form">
        <div class="form-group">
            <label for="name">Название товара</label>
            <input id="name" v-model="product.name" placeholder="Название товара" required />
        </div>

        <div class="form-group">
            <label for="seller">Продавец</label>
            <input id="seller" v-model="product.seller" placeholder="Продавец" required />
        </div>

        <div class="form-group">
            <label for="integration">Способ интеграции</label>
            <select  id="integration" v-model="product.integration" required>
                <option value="reels">Reels</option>
                <option value="stories">Stories</option>
            </select>
        </div>
        <button type="submit" class="btn btn-primary">Сохранить</button>
    </form>
</template>

<script lang="ts">
import { defineComponent, ref, watch } from 'vue';

export default defineComponent({
    props: {
        productToEdit: {
            type: Object,
            default: () => ({
                name: '',
                seller: '',
                integration: 'reels',
            }),
        },
    },
    emits: ['submit'],
    setup(props, { emit }) {
        const product = ref({ ...props.productToEdit });

        watch(() => props.productToEdit, (newVal) => {
            product.value = { ...newVal };
        });

        const submitForm = () => {
            emit('submit', product.value);
            product.value = { name: '', seller: '', integration: 'reels' };
        };

        return {
            product,
            submitForm,
        };
    },
});
</script>

<style lang="scss" scoped>
.product-form {
    display: flex;
    flex-direction: column;
    max-width: 500px;
    margin: 0 auto;
    padding: 20px;
    border: 1px solid #ddd;
    border-radius: 8px;
    background-color: #fff;

    .form-group {
        margin-bottom: 15px;

        label {
            display: block;
            font-weight: bold;
            margin-bottom: 5px;
        }

        input,
        select {
            width: 90%;
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 4px;
        }
    }

    .btn-primary {
        background-color: #007bff;
        color: #fff;
        padding: 10px 15px;
        border: none;
        border-radius: 4px;
        cursor: pointer;

        &:hover {
            background-color: #0056b3;
        }
    }
}
</style>