<template>
    <main>
        <div class="container-fluid">
            <div class="row">
                <div class="col-sm-3"></div>
                <div class="col-sm-6">
                    <div class="a-section">
                        <div class="a-spacing-top-medium"></div>
                        <h2 style="text-align: center">Updated product</h2>
                        <div class="a-spacing-top-medium"></div>

                        <!-- Category dropdown -->
                        <label>Category</label>
                         <select class="a-select-option" v-model="categoryID">
                            <option v-for="category in categories" :key="category._id" :value=category._id>{{ category.type }}</option>                       
                        </select>

                        <!-- Owner dropdown -->
                        <div class="a-spacing-top-medium"></div>
                        <label>Owner</label>
                         <select class="a-select-option" v-model="ownerID">
                            <option v-for="owner in owners" :key="owner._id"  :value=owner._id>{{ owner.name }}</option>                     
                        </select>

                        <!-- title -->
                        <div class="a-spacing-top-medium">
                            <label style="margin-bottom: 0px;">Title</label>
                            <input type="text" class="a-input-text" style="width: 100%" v-model="title" :placeholder="product.title"/>
                        </div>

                        <!-- price -->
                        <div class="a-spacing-top-medium">
                            <label>Price</label>
                            <input type="number" class="a-input-text" style="width: 100%" v-model="price" :placeholder="product.price"/>
                        </div>

                        <!-- stock quantity -->
                        <div class="a-spacing-top-medium">
                            <label>Stock quantity</label>
                            <input type="number" class="a-input-text" style="width: 100%" v-model="stockQuantity" :placeholder="product.stockQuantity"/>
                        </div>

                        <!-- description -->
                        <div class="a-spacing-top-medium">
                            <label>Description</label>
                            <textarea style="width: 100%" v-model="description" :placeholder="product.description"></textarea>
                        </div>

                        <!-- photo -->
                        <div class="a-spacing-top-medium">
                            <label>Add photo</label>
                            <div class="a-row a-spacing-top-medium">
                                <label class="choosefile-button">
                                    <i class="fal fa-plus"></i>
                                    <input type="file" style="width: 40%" @change="onFileSelected"/>
                                    <p style="margin-top: -70px">{{ fileName }}</p>
                                </label>
                            </div>
                        </div>

                        <!-- Button -->
                        <hr/>
                        <div class="a-spacing-top-large">
                            <span class="a-button-register">
                                <span class="a-button-inner">
                                    <span class="a-button-text" @click="onUpdateProduct">Update Product</span>
                                </span>
                            </span>
                        </div>
                    </div>
                </div>
                <div class="col-sm-3"></div>
                </div>
            </div>
    </main>
</template>

<script>
export default {
    async asyncData({ $axios, params }) {
        try {
            let categoriesAPICall = $axios.$get('http://localhost:3000/api/categories');
            let ownersAPICall = $axios.$get('http://localhost:3000/api/owners');
            let productAPICall = $axios.$get(`http://localhost:3000/api/products/${params.id}`);

            const [catResponse, ownResponse, productResponse] = await Promise.all([
                categoriesAPICall,
                ownersAPICall,
                productAPICall
            ])

            console.log(productResponse);

            return {
                categories: catResponse.categories,
                owners: ownResponse.owners,
                product: productResponse.product
            }
        } catch(err) {
            console.log(err);
        }
    },

    data() {
        return {
            categoryID: null,
            ownerID: null,
            title: "",
            price: "",
            stockQuantity: "",
            description: "",
            selectedFile: null,
            fileName: ""
        }
    },

    methods: {
        onFileSelected(event) {
            this.selectedFile = event.target.files[0],
            console.log(this.selectedFile);
            this.fileName = event.target.files[0].name
        },

        async onUpdateProduct() {
            let data = new FormData();
            data.append("title", this.title);
            data.append("price", this.price);
            data.append("description", this.description);
            data.append("categoryID", this.categoryID);
            data.append("ownerID", this.ownerID);
            data.append("photo", this.selectedFile, this.fileName);
            data.append("stockQuantity", this.stockQuantity);
            
            let result = await this.$axios.$put(`http://localhost:3000/api/products/${this.$route.params.id}`, data);

            this.$router.push('/');
        }
    }
}
</script>
