<template>
  <form-validation></form-validation>
</template>

<script>
import FormValidation from './components/FormValidation.vue'
import './index.css';

export default {
  name: 'App',
  components: {
    FormValidation
  }
}
</script>

<style>


#app {
  font-family: 'Inter', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  min-width: 320px;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  background-color: #111;

}
</style>
