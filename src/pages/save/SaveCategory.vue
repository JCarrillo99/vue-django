<template>

    <form action="" @submit.prevent="submit">
        <n-form-item label="Título" :feedback="errors.title" :validation-status=" errors.title == '' ? 'success' : 'warning' ">
            <n-input :status=" errors.title == '' ? 'success' : 'warning' " placeholder="Título" v-model:value="form.title" type="text" />
        </n-form-item>
        <n-form-item label="Url clean" :feedback="errors.url_clean" :validation-status=" errors.url_clean == '' ? 'success' : 'warning' ">
            <n-input :status=" errors.url_clean == '' ? 'success' : 'warning' " placeholder="Url clean" v-model:value="form.url_clean" type="text" />
        </n-form-item>
        <n-button class="mt-2" type="primary" attr-type="submite">Enviar</n-button>
    </form>
</template>

<script>
    export default {
        data(){
            return {
                category:"",
                form:{
                    title: "",
                    url_clean: "",
                },
                errors:{
                    title: "",
                    url_clean: "",
                },
            };
        },
        async mounted(){
            if(this.$route.params.id){
                // obtener categoría
                await this.getCategory()
                //inicializar
                this.initCategory()

            }
        },
        methods: {
            async getCategory(){
                this.category = await this.$axios
                    .get("http://localhost:8000/api/category/"+this.$route.params.id+"/?format=json")
                    // .then((res)=>{
                    //     console.log(res.data)
                    //     this.category = res.data
                    // })
                    this.category = this.category.data
            },
            initCategory(){
                this.form.title = this.category.title
                this.form.url_clean = this.category.url_clean
            },
            submit(){
                this.cleanForm()
                if(this.category == '')
                    this.$axios
                        .post("http://localhost:8000/api/category/",this.form)
                        .then((res)=>{
                            console.log(res.data)
                        })
                        .catch((error)=>{
                            if(error.response.data.title)
                                this.errors.title = error.response.data.title[0]
                            if(error.response.data.url_clean)
                                this.errors.url_clean = error.response.data.url_clean[0]
                        })
                    
                else
                    this.$axios
                        .put("http://localhost:8000/api/category/"+this.$route.params.id+"/?format=json", this.form)
                        .then((res)=>{
                            console.log(res.data)
                        })
                        .catch((error)=>{
                            if(error.response.data.title)
                                this.errors.title = error.response.data.title[0]
                            if(error.response.data.url_clean)
                                this.errors.url_clean = error.response.data.url_clean[0]
                        })

            },
            cleanForm(){
                this.errors.title = ''
                this.errors.url_clean = ''
            }
        }, 
        
    };
</script>