<template>
    <ToastNotificationStatus />
    <AdminLayout :logo="$page.props.assets.logo" :avatar="$page.props.assets.avatar">
        <div class="page-content">
            <div class="container-fluid">
                <div class="row">
                    <div class="col-12">
                        <div class="card">
                            <div class="card-body">
                                <div class="d-flex flex-wrap gap-1 align-items-end">
                                    <div>
                                        <label for="status-select" class="me-2 mt-2">ID:</label>
                                        <div class="me-3" style="max-width: 60px;">
                                            <input v-model="form.id" type="search" class="form-control my-1 my-lg-0"
                                                id="product_id" placeholder="ID...">
                                        </div>
                                    </div>
                                    <div>
                                        <label for="status-select" class="me-2 mt-2">Product Name:</label>
                                        <div class="me-3">
                                            <input v-model="form.name" type="search" class="form-control my-1 my-lg-0"
                                                id="product_name" placeholder="Search...">
                                        </div>
                                    </div>
                                    <div>
                                        <label for="status-select" class="me-2 mt-2">Status</label>
                                        <div class="me-sm-3">
                                            <select v-model="form.status" class="form-select my-1 my-lg-0"
                                                id="status-select">
                                                <option :value="'All'" selected>All</option>
                                                <option value="1">Active</option>
                                                <option value="0">Inactive</option>

                                            </select>
                                        </div>
                                    </div>

                                    <div>
                                        <label for="status-select" class="me-2 mt-2">Trending</label>

                                        <div class="me-sm-3">
                                            <select v-model="form.trending" class="form-select my-1 my-lg-0"
                                                id="status-select">
                                                <option :value="'All'" selected>All</option>
                                                <option value="1">Only trending</option>
                                                <option value="0">Only not trending</option>

                                            </select>
                                        </div>
                                    </div>

                                    <div>
                                        <label for="status-select" class="me-2 mt-2">Category</label>
                                        <div class="me-sm-3">
                                            <select v-model="form.category" class="form-select my-1 my-lg-0"
                                                id="status-select">
                                                <option selected :value="'All'">All</option>

                                                <option v-for="(category, index) in categories" :key="index"
                                                    :value="category.id" selected>{{ category.name }}</option>

                                            </select>
                                        </div>
                                    </div>

                                    <div>
                                        <label for="status-select" class="me-2 mt-2">Brand</label>
                                        <div class="me-sm-3">
                                            <select v-model="form.brand" class="form-select my-1 my-lg-0"
                                                id="status-select">
                                                <option selected :value="'All'">All</option>

                                                <option v-for="(brand, index) in brands" :key="index" :value="brand.id"
                                                    selected>{{ brand.name }}</option>

                                            </select>
                                        </div>
                                    </div>
                                    <div>

                                        <button @click="filterProducts"
                                            class="btn btn-info waves-effect waves-light"><i
                                                class="mdi mdi-basket me-1"></i>Filter</button>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="row">
                    <div class="col-12">
                        <div class="card">
                            <div class="card-body">
                                <div class="row mb-2 align-items-end">
                                    <div class="col-lg-8">

                                    </div>
                                    <div class="col-lg-4">
                                        <div class="text-lg-end">
                                            <Link as="button" :href="route('admin.products.create')"
                                                class="btn btn-danger waves-effect waves-light me-2"><i
                                                class="mdi mdi-basket me-1"></i> Add New Product</Link>
                                            <a type="button" :href="route('admin.products.export')"
                                                class="btn btn-light waves-effect">Export</a>
                                        </div>
                                    </div><!-- end col-->
                                </div>

                                <div class="table-responsive">
                                    <table class="table table-centered table-nowrap mb-0">
                                        <thead class="table-light">
                                            <tr>
                                                <th>Id</th>
                                                <th>Category</th>
                                                <th>Name</th>
                                                <th>Slug</th>
                                                <th>Brand</th>
                                                <th>Description</th>
                                                <th>Quantity</th>
                                                <th>Status</th>
                                                <th>Images</th>
                                                <th>Trending</th>
                                                <th>Meta Title</th>
                                                <th style="width: 125px;">Action</th>
                                            </tr>
                                        </thead>
                                        <tbody>
                                            <ProductListItem v-for="(product, index) in products.data" :key="index"
                                                :product="product" />
                                        </tbody>
                                    </table>
                                </div>
                                <Pagination class="mt-6" :links="products.links" />
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </AdminLayout>
</template>

<script setup>
import AdminLayout from '@/Layouts/AdminLayout.vue';
import { Link, usePage } from '@inertiajs/inertia-vue3';
import { reactive, ref } from 'vue';
import ProductListItem from '@/Components/Admin/Products/ProductListItem.vue'
import Pagination from '@/Components/Admin/AdminPagination.vue';
import { Inertia } from '@inertiajs/inertia';
import ToastNotificationStatus from '@/Components/Admin/ToastNotificationStatus.vue';


const form = reactive({
    id: usePage().props.value.ziggy?.query?.search_id ? usePage().props.value.ziggy?.query?.search_id : '',
    name: usePage().props.value.ziggy?.query?.name ? usePage().props.value.ziggy?.query?.name : '',
    status: usePage().props.value.ziggy?.query?.status ? usePage().props.value.ziggy?.query?.status : 'All',
    trending: usePage().props.value.ziggy?.query?.trending ? usePage().props.value.ziggy?.query?.trending : 'All',
    brand: usePage().props.value.ziggy?.query?.brand ? usePage().props.value.ziggy?.query?.brand : 'All',
    category: usePage().props.value.ziggy?.query?.category ? usePage().props.value.ziggy?.query?.category : 'All',
})


const props = defineProps({

    products: {
        type: Object,
        required: true,
    },
    categories: {
        type: Object,
        required: true,
    },
    brands: {
        type: Object,
        required: true,
    }

});


const filterProducts = () => {

    Inertia.get(route('admin.products.index'), {
        id: form.id,
        name: form.name,
        status: form.status,
        trending: form.trending,
        brand: form.brand,
        category: form.category,
    },
        {
            preserveState: true,
        });
}

const exportProducts = () => {
    Inertia.get(route('admin.products.export'));
}
</script>

<style lang="scss">

</style>
