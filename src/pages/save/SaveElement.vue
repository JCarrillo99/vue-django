<template>

    <form action="" @submit.prevent="submit">
        <n-form-item label="Título" :feedback="errors.title" :validation-status=" errors.title == '' ? 'success' : 'warning' ">
            <n-input :status=" errors.title == '' ? 'success' : 'warning' " placeholder="Título" v-model:value="form.title" type="text" />
        </n-form-item>
        <n-form-item label="Url clean" :feedback="errors.url_clean" :validation-status=" errors.url_clean == '' ? 'success' : 'warning' ">
            <n-input :status=" errors.url_clean == '' ? 'success' : 'warning' " placeholder="Url clean" v-model:value="form.url_clean" type="text" />
        </n-form-item>
        <n-form-item label="Descripción" :feedback="errors.description" :validation-status=" errors.description == '' ? 'success' : 'warning' ">
            <!-- <n-input :status=" errors.description == '' ? 'success' : 'warning' " placeholder="Descripción" v-model:value="form.description" type="textarea" /> -->
            <ckeditor :editor="editor.editor" v-model="form.description"></ckeditor>
        </n-form-item>


        <n-form-item label="Categorías" :feedback="errors.category_id" :validation-status=" errors.category_id == '' ? 'success' : 'warning' ">
            <n-select :status=" errors.category_id == '' ? 'success' : 'warning' " v-model:value="form.category_id" :options="categories_options" type="textarea" />
        </n-form-item>
        <n-form-item label="Tipo" :feedback="errors.type_id" :validation-status=" errors.type_id == '' ? 'success' : 'warning' ">
            <n-select :status=" errors.type_id == '' ? 'success' : 'warning' " v-model:value="form.type_id" :options="tipes_options" type="textarea" />
        </n-form-item>
        <n-button class="mt-2" type="primary" attr-type="submite">Enviar</n-button>
    </form>
</template>

<script>
     import CKEditor from "@ckeditor/ckeditor5-build-classic"
    export default {
        data(){
            return {
                editor:{
                    editor: CKEditor
                },
                categories_options:[],
                tipes_options:[],
                element:"",
                form:{
                    title: "",
                    url_clean: "",
                    description: "",
                    category_id: "",
                    type_id: "",
                },
                errors:{
                    title: "",
                    url_clean: "",
                    description: "",
                    category_id: "",
                    type_id: "",
                },
            };
        },
        async mounted(){
            if(this.$route.params.id){
                // obtener categoría
                await this.getElement()
                //inicializar
                this.initElement()


            }
            this.categories()
            this.tipes()
        },
        methods: {
            async getElement(){
                this.element = await this.$axios
                    .get("http://localhost:8000/api/element/"+this.$route.params.id+"/?format=json")
                    this.element = this.element.data
                    console.log(this.element)
            },
            initElement(){
                this.form.title = this.element.title
                this.form.url_clean = this.element.url_clean
                this.form.description = this.element.description
                this.form.category_id = this.element.category.id
                this.form.type_id = this.element.type.id
            },

            categories(){
                this.$axios
                .get('http://localhost:8000/api/category/all/?format=json')
                .then((res)=>{
                    this.categories_options = res.data.map((c)=>{
                        return {
                            label: c.title,
                            value: c.id
                        }
                    })
                })
            },
            tipes(){
                this.$axios
                .get('http://localhost:8000/api/type/all/?format=json')
                .then((res)=>{
                    this.tipes_options = res.data.map((t)=>{
                        return {
                            label: t.title,
                            value: t.id
                        }
                    })
                })
            },
            

            submit(){
                this.cleanForm()
                if(this.element == '')
                    this.$axios
                        .post("http://localhost:8000/api/element/",this.form)
                        .then((res)=>{
                            console.log(res.data)
                        })
                        .catch((error)=>{
                            if(error.response.data.title)
                                this.errors.title = error.response.data.title[0]
                            if(error.response.data.url_clean)
                                this.errors.url_clean = error.response.data.url_clean[0]
                            if(error.response.data.description)
                                this.errors.description = error.response.data.description[0]
                            if(error.response.data.category_id)
                                this.errors.category_id = error.response.data.category_id[0]
                            if(error.response.data.type_id)
                                this.errors.type_id = error.response.data.type_id[0]
                        })
                    
                else
                    this.$axios
                        .put("http://localhost:8000/api/element/"+this.$route.params.id+"/?format=json", this.form)
                        .then((res)=>{
                            console.log(res.data)
                            console.log(this.form)
                        })
                        .catch((error)=>{
                            console.log(error.response.data)

                            if(error.response.data.title)
                                this.errors.title = error.response.data.title[0]
                            if(error.response.data.url_clean)
                                this.errors.url_clean = error.response.data.url_clean[0]
                            if(error.response.data.description)
                                this.errors.description = error.response.data.description[0]
                            if(error.response.data.category_id)
                                this.errors.category_id = error.response.data.category_id[0]
                            if(error.response.data.type_id)
                                this.errors.type_id = error.response.data.type_id[0]
                        })

            },
            cleanForm(){
                this.errors.title = ''
                this.errors.url_clean = ''
                this.errors.description = ''
                this.errors.category_id = ''
                this.errors.type_id = ''
            }
        }
        
    };
</script>