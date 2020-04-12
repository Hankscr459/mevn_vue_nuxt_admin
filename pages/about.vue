<template>
  <div>
    <h1>{{ name }}</h1>
    <input v-model="name" />
    <button @click="onclick">Click me</button>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        name: 'about me im awesome'
      }
    },
    methods: {
      onclick() {
        alert(this.name)
      }
    }
  }
</script>
