<script setup lang='ts'>
import { ref, onMounted, onUpdated } from 'vue'
import { NResult, NButton } from 'naive-ui'
import { t } from '@/locales';
import { get } from '@/utils/request';
import Product from './Product.vue'
const htmlContent = ref('')
const loading = ref(true)
const products = ref([])
async function getProduct(): Promise<void> {
    try {

        const data = {
            'category': 'مبيدات-حشرية'
        }

        // const data = {
        //     'category': 'Clothing'
        // }
        const response = await get<any>({
            url: 'product-category/',
            data
        })
        products.value = response
        htmlContent.value = response
        console.log("plant", response)
        loading.value = false
    } catch (error: any) {
        console.log("error", error)
        throw error;
        loading.value = false
    }
}
onMounted(async () => {
    getProduct();
})

onUpdated(async () => {
    getProduct();
})
</script>
<template>


    <div v-if="loading" class="flex flex-col justify-center items-center min-h-[50vh]">
        <div class="spinner mt-8"></div>
    </div>


    <div v-if="!loading && !products" class="p-4 rounded w-full flex flex-col text-base min-h-[50vh]">
        <NResult status="info" :title="t('common.sorry')" description="Not found any product">
            <template #footer>
                <NButton @click="getProduct()" type="primary">
                    {{ t('common.back') }}
                </NButton>
            </template>

        </NResult>
    </div>

    <div v-else class=" grid grid-cols-2 md:grid-cols-3 gap-5">
        <template v-for="(product, index) in products">

            <Product :row="product" />
        </template>
    </div>


</template>
