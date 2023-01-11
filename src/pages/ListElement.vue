<template>
    <h1>Listado Elementos</h1>

    <!-- <a href="http://localhost:8080/type">Tipos</a> -->
    <!-- <router-link to="http://localhost:8080/type/">Tipos</router-link> -->
    <router-link :to="{ name: 'list-element' }">Elementos</router-link>

    <n-table :bordered="false" class="my-table">
        <thead>
            <tr>
                <th>ID</th>
                <th>Título</th>
                <th>Acciones</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="e in elements" :key="e.id">
                <td>{{ e.id }}</td>
                <td>{{ e.title }}</td>
                <td>
                    <n-button type="primary">
                        <router-link :to="{ name: 'detail-element', params:{ slug: e.url_clean } }">Ver</router-link>
                    </n-button>
                </td>
            </tr>
        </tbody>
    </n-table>


</template>

<script>
    export default {
        data(){
            return {
                elements:[]
            }
        },
        mounted(){

            let url = 'http://localhost:8000/api/element/?format=json';

            if(this.$route.params.type == "c")
                url = 'http://localhost:8000/api/category/'+this.$route.params.id+'/elements/?format=json';

            else if(this.$route.params.type == "t")
                url = 'http://localhost:8000/api/type/'+this.$route.params.id+'/elements/?format=json';



            this.$axios
            .get(url)
            .then((res)=>{
                console.log(res.data);
                this.elements = res.data.results ? res.data.results : res.data;
            })
        }
    }
</script>