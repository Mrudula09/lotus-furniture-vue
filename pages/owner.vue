<template>
    <main>
        <div class="container-fluid">
            <div class="row">
                <div class="col-sm-3"></div>
                <div class="col-sm-6">
                    <div class="a-section">
                        <div class="a-spacing-top-medium"></div>
                        <h2 style="text-align: center">Add a new Owner</h2>
                        <div class="a-spacing-top-medium"></div>
                        <form>
                            <!-- Name -->
                            <label>Name</label>
                            <input type="text" class="a-input-text" style="width: 100%" v-model="name"/>

                            <!-- About -->
                            <label>About</label>
                            <input type="text" class="a-input-text" style="width: 100%" v-model="about"/>

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
                                        <span class="a-button-text" @click="onAddOwner">Add Owner</span>
                                    </span>
                                </span>
                            </div>
                        </form>

                        <br/>
                        <ul v-for="owner in owners" :key="owner._id" class="list-group-item">
                            <li>{{ owner.name }}</li>
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
            let response = await $axios.$get('http://localhost:3000/api/owners'); 
            return {
                owners: response.owners
            }
        } catch(err) {
            console.log(err);
        }
    },
    
    data() {
        return {
            name: "",
            about: "",
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

        async onAddOwner() {
            let data = new FormData();
            data.append("name", this.name);
            data.append("about", this.about);
            data.append("photo", this.selectedFile, this.fileName);
            let result = await this.$axios.$post('http://localhost:3000/api/owners', data);

            this.owners.push({ name: this.name });
        }
    }
}
</script>

