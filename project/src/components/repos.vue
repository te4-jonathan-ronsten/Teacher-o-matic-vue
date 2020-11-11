<template>
  <input type="text" v-model="input" name="input">
  <button @Click="search">search</button>
</template>

<script>
export default {
    name: "Repos",
    props: ['Template'],
    data() {
        return {
            input: ""
        }
    },
    methods: {
        search() {
         this.$emit('search-repos', this.input)
        }
    }
    }
</script>