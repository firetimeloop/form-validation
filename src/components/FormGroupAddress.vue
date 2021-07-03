<template>
      <div class="form__group" >
      <h2>Адрес:</h2>

      <div class="form__field">
        <label class="form__label" for="post-index">Индекс:</label>
        <input class="form__input" @input="$emit('update:postIndex', $event.target.value)" id="post-index" type="number">        
      </div>

      <div class="form__field">
        <label class="form__label" for="country">Страна:</label>
        <input class="form__input" @input="$emit('update:country', $event.target.value)" id="country">        
      </div>      


      <div class="form__field">
        <label class="form__label" for="region">Область:</label>
        <input class="form__input" @input="$emit('update:region', $event.target.value)" id="region">        
      </div>


      <div class="form__field">
        <label class="form__label" for="city">Город:</label>
        <input @change="cityErrorValidate(cityError)" :class="cityError.$error ? 'form__input_not-valide' : ''" class="form__input" @input="$emit('update:city', $event.target.value)" id="city">        
      </div>

      <p v-if="cityError.$error" class="form__error-message">Поле Город обязательно для заполнения.</p>

      <div class="form__field">
        <label class="form__label" for="street">Улица:</label>
        <input class="form__input" @input="$emit('update:street', $event.target.value)" id="street">        
      </div>


      <div class="form__field">
        <label class="form__label" for="house">Дом:</label>
        <input class="form__input" @input="$emit('update:house', $event.target.value)" id="house">        
      </div>

    </div>    
</template>

<script>
export default {
    name: 'FormGroupAddress',
    props: {
      cityError: Object,
      cityErrorValidate: Function},
    emits: ['update:postIndex', 'update:country', 'update:region', 'update:city', 'update:street', 'update:house'],
}
</script>

<style>

</style>