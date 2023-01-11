<template>
    <div v-if="element != Object">
        <h1 class="text-4xl">{{element.type.title}}</h1>
        <p class="text-sm">{{ element.description}}</p>
    </div>
</template>

<script>
export default {
    data(){
        return{
            element:Object,
        };
    },
    mounted(){
        this.$axios
            .get('http://localhost:8000/api/element/url/?url_clean='+this.$route.params.slug+'&format=json')
            .then((res)=>{
                console.log(res.data)
                this.element = res.data
            })
    }
}
</script>