<template>
    <div class="row justify-content-center">
        <div class="col-md-8">
            <div class="card">
                <div class="card-header">Form</div>

                <div class="card-body">
                    <!-- <form> -->
                        <!-- @csrf -->
                        <div class="form-group row">
                            <label for="name" class="col-md-4 col-form-label text-md-right">Search</label>
                            <div class="col-md-6">
                                <input id="search" v-model="search" @change="searchVal()" type="text" class="form-control" name="Search" required autocomplete="name" autofocus>
                            </div>
                        </div>

                        <div class="form-group row mb-0">
                            <div class="col-md-6 offset-md-4">
                                <button type="submit" @click="fetch()" class="btn btn-primary">
                                    Search
                                </button>
                            </div>
                        </div>

                        <div v-for="result in results" :key="result.id.videoId">
                            <div @click="getVideoInfo(result)">
                                <img :src="result.snippet.thumbnails.default.url" class="img-rounded" alt="Cinque Terre" style="padding: 12px;" />
                            </div>
                        </div>
                    <!-- </form> -->
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        mounted() {
            console.log('FormComponent mounted.')
        },

        // Properties returned from data() become reactive state
        // and will be exposed on `this`.
        data() {
            return {
                results: [],
                search: '',
                apiUrl: 'api/fetch'
            }
        },

        methods:{

            searchVal() {
                console.log(this.search);
            },
            
            fetch(){
                let self= this
                let payload = {
                    search: this.search
                }
                axios.post(`${this.apiUrl}`, payload).then(function (response) {
                    self.results= response.data
                    console.log(self.results)
                })
                .catch(function (error) {
                    console.log(error)
                })
                .then(function () {
                    // always executed
                })
            },

            create(){
                let self= this
                let payload={
                    name: this.category.name
                }
                axios.post(`${this.apiUrl}`, payload).then(function (response) {
                    if(response.data.error == false){
                        self.category.name= null
                        self.fetchCategories()
                        self.$toastr.s("Successfully created");
                        $('#addCategoryModal').modal('hide')
                    }
                })
                .catch(function (error) {
                    console.log(error)
                })
                .then(function () {
                    // always executed
                })
            },

        }
    }
</script>
