<template>
    <n-menu :options="options"> </n-menu>
</template>

<script>
import { h } from "vue";
import { RouterLink } from "vue-router";

export default {
  name: "App",
  data() {
    return {
      options: [],
    };
  },
  mounted() {
    this.elements()
    this.categories()
    this.type()
  },
  methods:{
    elements(){
        this.$axios
        .get('http://localhost:8000/api/element/?format=json')
        .then((res)=>{
            const optionElement = [
              {
                label: () =>
                  h(
                    RouterLink,
                    {
                      to: {
                        name: "list-element",
                      },
                    },
                    { default: () => "Listado Elementos" }
                  ),
                key: "list-element",
              },
            ];
            res.data.results.forEach(c => {
              optionElement.push({
                label: c.title,
                key: "c " + c.id,
              });
            });
            // console.log(optionElement)
            this.options.push({
              label: "Elementos",
              key: "3 parent",
              children: optionElement
            })
        })
    },
    categories(){
        this.$axios
        .get('http://localhost:8000/api/category/?format=json')
        .then((res)=>{
            const optionCate = [
              {
                label: () =>
                  h(
                    RouterLink,
                    {
                      to: {
                        name: "list-category",
                      },
                    },
                    { default: () => "Listado Categoría" }
                  ),
                key: "list-category",
              },
            ];
            res.data.results.forEach(c => {
              optionCate.push({
                label: c.title,
                key: "c " + c.id,
              });
            });
            // console.log(optionCate)
            this.options.push({
              label: "Categorías",
              key: "1 parent",
              children: optionCate
            })
        })
    },
    type(){
        this.$axios
        .get('http://localhost:8000/api/type/?format=json')
        .then((res)=>{
            const optionType = [
              {
                label: () =>
                  h(
                    RouterLink,
                    {
                      to: {
                        name: "list-type",
                      },
                    },
                    { default: () => "Listado Tipos" }
                  ),
                key: "list-type",
              },
            ];
            res.data.results.forEach(t => {
              optionType.push({
                label: t.title,
                key: "t " + t.id,
              });
            });
            // console.log(optionCate)
            this.options.push({
              label: "Type",
              key: "2 parent",
              children: optionType
            })
        })
    }
  }
};
</script>