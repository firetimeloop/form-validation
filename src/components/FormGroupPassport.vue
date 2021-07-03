<template>
      <div class="form__group" >
      <h2>Паспорт:</h2>

      <div class="form__field">
        <label class="form__label" for="passport-type">Тип документа:</label>
        <select @change="passportErrorValidate(passportError)" :class="passportError.$error ? 'form__input_not-valide' : ''" class="form__input" @input="$emit('update:passportType', $event.target.value)" id="passport-type">
          <option value="passport">Паспорт</option>
          <option value="birth-certificate">Свидетельство о рождении</option>
          <option value="drive-license">Вод. удостоверение</option>
        </select>        
      </div>

      <p v-if="passportError.$error" class="form__error-message">Поле Тип документа для заполнения.</p>


      <div class="form__field">
        <label class="form__label" for="passport-serie">Серия:</label>
        <input class="form__input" @input="$emit('update:passportSerie', $event.target.value)" id="passport-serie" type="number">        
      </div>      


      <div class="form__field">
        <label class="form__label" for="passport-number">Номер:</label>
        <input class="form__input" @input="$emit('update:passportNumber', $event.target.value)" id="region" type="number">        
      </div>


      <div class="form__field">
        <label class="form__label" for="passport-issued-by">Кем выдан:</label>
        <input class="form__input" @input="$emit('update:passportIssuedBy', $event.target.value)" id="passport-issued-by">        
      </div>


      <div class="form__field">
        <label class="form__label" for="passport-issued-date">Дата выдачи:</label>
        <input @change="passportErrorValidate(passportDateError)" :class="passportDateError.$error ? 'form__input_not-valide' : ''" class="form__input" @input="$emit('update:passportIssuedDate', $event.target.value)" id="passport-issued-date" type="date">        
      </div>      
      <p v-if="passportDateError.$error" class="form__error-message">Поле Дата выдачи обязательно для заполнения.</p>
    </div>
</template>

<script>
export default {
    name: 'FormGroupPassport',
    props: {
      passportError: Object,
      passportDateError: Object,
      passportErrorValidate: Function
    },
    emits: ['update:passportType', 'update:passportSerie', 'update:passportNumber', 'update:passportIssuedBy', 'update:passportIssuedDate'],
}
</script>

<style>

</style>