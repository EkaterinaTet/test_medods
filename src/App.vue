<template>
  <div class="container">
    <h1>Создание клиента</h1>
    <p>* поля обязательны для заполнения</p>
    <ClientForm />
  </div>
</template>

<script>
import ClientForm from "./components/ClientForm.vue";

export default {
  name: "App",
  components: {
    ClientForm,
  },
};
</script>

<style lang="sass">
html
  box-sizing: border-box

*, *::after, *::before
  box-sizing: inherit
body
 font-size: 18px
h2, h1
 text-align: center
span
 font-size: 14px
 color: #bb3f3f
 max-width: 300px
button
 width:100px
 height: 50px
 display: block
 margin: 0 auto
 cursor: pointer
 font-size: 16px
 &:hover
  background-color:#d1c4c4
  transition: all 0.3s ease-in
input, select
 padding: 8px
 font-size: 16px
select
 cursor: pointer
.container
  max-width: 1200px
  margin: 0 auto
</style>
