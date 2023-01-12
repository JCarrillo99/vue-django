<template>
    <h1>Listado Categorías</h1>

    <!-- <a href="http://localhost:8080/type">Tipos</a> -->
    <!-- <router-link to="http://localhost:8080/type/">Tipos</router-link> -->
    <n-button>
        <router-link :to="{ name: 'save-category' }">Crear</router-link>
    </n-button>


    <n-table :bordered="false" class="my-table">
        <thead>
            <tr>
                <th>Título</th>
                <th>Acciones</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="c in categories" :key="c.id">
                <td>{{ c.title }}</td>
                <td>
                    <n-button>
                        <router-link :to="{ name: 'list-element', params:{ type:'c', id:c.id} }">Elementos</router-link>
                    </n-button>
                    <n-button>
                        <router-link :to="{ name: 'save-category', params:{ id:c.id} }">Editar</router-link>
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
                categories:[]
            }
        },
        mounted(){
            this.$axios
            .get('http://localhost:8000/api/category/?format=json')
            .then((res)=>{
                // console.log(res.data);
                this.categories = res.data.results
            })
        }
    }
</script>