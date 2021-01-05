<template>
    <main>
        <div class="container-fluid">
            <div class="row">
                <div class="col-sm-3"></div>
                <div class="col-sm-6">
                    <div class="a-section">
                        <div class="a-spacing-top-medium"></div>
                        <h2 style="text-align: center">Add a new Category</h2>
                        <div class="a-spacing-top-medium"></div>
                        <form>
                            <!-- Type -->
                            <label>Type</label>
                            <input type="text" class="a-input-text" style="width: 100%" v-model="type"/>

                            <!-- Button -->
                            <hr/>
                            <div class="a-spacing-top-large">
                                <span class="a-button-register">
                                    <span class="a-button-inner">
                                        <span class="a-button-text" @click="onAddCategory">Add Category</span>
                                    </span>
                                </span>
                            </div>
                        </form>

                        <br/>
                        <ul v-for="category in categories" :key=category._id class="list-group-item">
                            <li>{{ category.type }}</li>
                        </ul>
                    </div>
                </div>
                <div class="col-sm-3"></div>
            </div>
        </div>
    </main>
</template>

<script>
export default {

    async asyncData({ $axios }) {
        try {
            let response = await $axios.$get('http://localhost:3000/api/categories'); 
            return {
                categories: response.categories
            }
        } catch(err) {
            console.log(err);
        }
    },
    
    data() {
        return {
            type: ""
        }
    },

    methods: {
        async onAddCategory() {
            let data = { type: this.type };
            let result = await this.$axios.$post('http://localhost:3000/api/categories', data);

            this.categories.push(data);
        }
    }
}
</script>

