<template>
    <div class="product-list">
        <ProductForm @submit="addOrUpdateProduct" :productToEdit="productToEdit" />

        <FilterSort @filter="filterProducts" @sort="sortProducts" />

        <div class="product-items">
            <ProductItem v-for="product in paginatedProducts" :key="product.id" :product="product" @edit="editProduct"
                @delete="deleteProduct" />
        </div>

        <Pagination :total="filteredProducts.length" :currentPage="currentPage" @change="changePage" />
    </div>
</template>

<script lang="ts">
import { defineComponent, ref, computed, onMounted, watch } from 'vue';
import ProductForm from '../components/ProductForm.vue';
import ProductItem from '../components/ProductItem.vue';
import Pagination from '../components/Pagination.vue';
import FilterSort from '../components/FilterSort.vue';

interface Product {
    id: number;
    name: string;
    seller: string;
    integration: string;
}

export default defineComponent({
    components: {
        ProductForm,
        ProductItem,
        Pagination,
        FilterSort,
    },
    setup() {
        const products = ref<Product[]>([]);
        const productToEdit = ref<Product | null>(null);
        const currentPage = ref(1);
        const filterBy = ref('');
        const sortOrder = ref('asc');

        onMounted(() => {
            const savedProducts = localStorage.getItem('products');
            if (savedProducts) {
                products.value = JSON.parse(savedProducts);
            }
        });

        watch(products, (newProducts) => {
            localStorage.setItem('products', JSON.stringify(newProducts));
        }, { deep: true });

        const addOrUpdateProduct = (product: Product) => {
            if (productToEdit.value) {
                const index = products.value.findIndex((p) => p.id === productToEdit.value?.id);
                if (index !== -1) {
                    products.value[index] = { ...product, id: productToEdit.value.id };
                }
                productToEdit.value = null;
            } else {
                products.value.push({ ...product, id: Date.now() });
            }
        };

        const editProduct = (product: Product) => {
            productToEdit.value = product;
        };

        const deleteProduct = (id: number) => {
            products.value = products.value.filter((product) => product.id !== id);
        };

        const filterProducts = (integration: string) => {
            filterBy.value = integration;
            currentPage.value = 1;
        };

        const sortProducts = (order: string) => {
            sortOrder.value = order;
        };

        const filteredProducts = computed(() => {
            return products.value.filter((product) => {
                return filterBy.value === '' || product.integration === filterBy.value;
            });
        });

        const sortedProducts = computed(() => {
            return filteredProducts.value.sort((a, b) => {
                if (sortOrder.value === 'asc') {
                    return a.name.localeCompare(b.name);
                } else {
                    return b.name.localeCompare(a.name);
                }
            });
        });

        const paginatedProducts = computed(() => {
            const start = (currentPage.value - 1) * 20;
            return sortedProducts.value.slice(start, start + 20);
        });

        const changePage = (page: number) => {
            currentPage.value = page;
        };

        return {
            products,
            productToEdit,
            currentPage,
            filterBy,
            sortOrder,
            paginatedProducts,
            filteredProducts,
            addOrUpdateProduct,
            editProduct,
            deleteProduct,
            filterProducts,
            sortProducts,
            changePage,
        };
    },
});
</script>

<style lang="scss" scoped>
.product-list {
    max-width: 800px;
    margin: 0 auto;
}

.product-items {
    display: flex;
    align-items: center;
    justify-content: flex-start;
    flex-wrap: wrap;
    max-width: 800px;
    gap: 16px;
}
</style>