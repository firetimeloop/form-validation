<template>
  <form class="form">
    <h1 class="form__header">Форма создания Клиента</h1>
    <div class="form__group" v-if="currentPage === 1">     
      <div class="form__field">
        <label class="form__label" for="surname">Фамилия:</label>
        <input class="form__input" @change="validateFormField(v$.form.surname)" :class="v$.form.surname.$error ? 'form__input_not-valide' : ''" v-model="form.surname" id="surname">        
      </div>
      <p v-if="v$.form.surname.$error" class="form__error-message">Поле Фамилия обязательно для заполнения.</p>

      <div class="form__field">
        <label class="form__label" for="name">Имя:</label>
        <input class="form__input" @change="validateFormField(v$.form.name)" :class="v$.form.name.$error ? 'form__input_not-valide' : ''" v-model="form.name" id="name">
      </div>      
      <p v-if="v$.form.name.$error" class="form__error-message">Поле Имя обязательно для заполнения.</p>

      <div class="form__field">
        <label class="form__label" for="patronymic">Отчество:</label>
        <input class="form__input" v-model="form.patronymic" id="patronymic">        
      </div>


      <div class="form__field">
        <label class="form__label" for="birth-date">Дата рождения:</label>
        <input class="form__input" @change="validateFormField(v$.form.birthDate)" :class="v$.form.birthDate.$error ? 'form__input_not-valide' : ''" v-model="form.birthDate" id="birth-date" type="date">        
      </div>
      <p v-if="v$.form.birthDate.$error" class="form__error-message">Поле Дата рождения обязательно для заполнения.</p>

      <div class="form__field">
        <label class="form__label" for="phone-number">Номер телефона:</label>
        <input class="form__input" @change="validateFormField(v$.form.phoneNumber)" :class="v$.form.phoneNumber.$error ? 'form__input_not-valide' : ''" v-model="form.phoneNumber" id="phone-number" type="tel">        
      </div>
      <p v-if="v$.form.phoneNumber.$error" class="form__error-message">Поле Номер телефона обязательно для заполнения (номер должен состоять из 11 цифр и начинаться с 7).</p>

      <div class="form__field">
        <p class="form__label">Пол:</p>
        <input class="form__input_radio" v-model="form.gender" name='gender' id="gender1" type="radio" value="male">
        <label class="form__label_radio" for="gender1">Мужской</label>
        <input class="form__input_radio" v-model="form.gender" name='gender' id="gender2" type="radio" value="female">
        <label class="form__label_radio" for="gender2">Женский</label>        
      </div>
      
      <div class="form__field">
        <label class="form__label" for="clients-group">Группа клиентов:</label>
        <select @change="validateFormField(v$.form.clientGroup)" :class="v$.form.clientGroup.$error ? 'form__input_not-valide' : ''" class="form__input" v-model="form.clientGroup" multiple id="clients-group" >
          <option value="vip">VIP</option>
          <option value="troublesome">Проблемные</option>
          <option value="obligatory">ОМС</option>
        </select>        
      </div>
      <p v-if="v$.form.clientGroup.$error" class="form__error-message">Поле Группа клиентов обязательно для заполнения.</p>

      <div class="form__field">
        <label class="form__label" for="attending-doctor">Лечащий врач:</label>
        <select class="form__input" v-model="form.attendingDoctor"  id="attending-doctor">
          <option value="ivanov">Иванов</option>
          <option value="zaharov">Захаров</option>
          <option value="chernysheva">Чернышева</option>
        </select>        
      </div>

      <div class="form__field">
        <input v-model="form.isSendSms" name='send-sms' id="send-sms" type="checkbox" class="form__input_checkbox">
        <label class="form__label_checkbox" for="send-sms">Не отправлять СМС</label>        
      </div>

    </div>  

    <form-group-address v-if="currentPage === 2"
    :cityError="v$.form.city"
    :cityErrorValidate="validateFormField"
    v-model:postIndex="form.postIndex"
    v-model:country="form.country"
    v-model:region="form.region"
    v-model:city="form.city"
    v-model:street="form.street"
    v-model:house="form.house"></form-group-address>

    <form-group-passport v-if="currentPage === 3"
    :passportError="v$.form.passportType"
    :passportDateError="v$.form.passportIssuedDate"
    :passportErrorValidate="validateFormField"
    v-model:passportType="form.passportType"
    v-model:passportSerie="form.passportSerie"
    v-model:passportNumber="form.passportNumber"
    v-model:passportIssuedBy="form.passportIssuedBy"
    v-model:passportIssuedDate="form.passportIssuedDate"></form-group-passport>

    <div class="form__group" v-if="currentPage === 4">
      <h2>Форма заполнена, спасибо!</h2>
    </div>
    <div class="form__group form__group_pad" v-if="currentPage !== 4">
      <p v-if="v$.$error" class="form__error-message">Красные поля перед отправкой должны быть заполнены. Ранее веденные данные сохранены.</p>
      <button @click.prevent="currentPage--" class="form__button" :disabled="currentPage === 1">Назад</button>
      <button v-if="currentPage !== 3" @click.prevent="currentPage++" class="form__button">Далее</button>
      <button v-if="currentPage === 3" @click.prevent="submitForm" class="form__button">Отправить</button>
    </div>
  </form>
</template>

<script>

import FormGroupAddress from './FormGroupAddress.vue'
import FormGroupPassport from './FormGroupPassport.vue'
import useValidate from '@vuelidate/core'
import { required, helpers } from '@vuelidate/validators'

const correctNumber = (value) => {
  return (Number(value) ?? false) && value.length === 11 && value[0] === "7"
}

export default {
  name: 'FormValidation',
  components: {
    FormGroupAddress, FormGroupPassport
  },
  data() {
    return {
      v$: useValidate(),
      form: {
        surname: null,
        name: null,        
        patronymic: null,
        birthDate: null,        
        phoneNumber: null,
        gender: null,        
        clientGroup: [],
        attendingDoctor: null,        
        isSendSms: false,
        postIndex: null,        
        country: null,
        region: null,
        city: null,
        street: null,
        house: null,
        passportType: null,
        passportSerie: null,
        passportNumber: null,
        passportIssuedBy: null,
        passportIssuedDate: null
      },
      currentPage: 1,
    }
  },
  methods: {
    validateFormField(field){
      field.$touch()
    },
    validateAll(){
      this.v$.$validate()
    },
    submitForm(){
      this.validateAll()
      if (!this.v$.$error){
        this.currentPage++
      }
    }
  },
  validations() {
    return {
      form: {
        surname: { required },
        name: { required },
        birthDate: { required },
        phoneNumber: { required, correctNumber: helpers.withMessage(
          'Number with 11 digit which starts with 7',
          correctNumber
        )},
        clientGroup: { required },
        city: { required },
        passportType: { required },
        passportIssuedDate: { required },
      }
    }
  }
}
</script>

<style>
.form {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 95%;
  
}

.form__header{
  color: #ddd;
}

.form__group{
  border: 1px solid #000;
  border-radius: 50px;
  padding: 5%;
  background-color: #fff;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  width: 75%;
}

.form__group_pad{
  flex-direction: row;
}

.form__button{
  width: 50%;
  height: 30px;
}

.form__label{
  display: inline-block;
  width: 25%;
  text-align: left;
}

.form__label_radio{
  width: 37.5%;
}

.form__label_checkbox{
  width: 75%;
  margin-left: 25%;
}

.form__label_radio, .form__label_checkbox {
  border: 1px solid black;
  display: inline-block;
  text-align: center;
  padding: 10px;
}

.form__input_radio, .form__input_checkbox {
  display: none;
}

.form__input_radio:checked + label, .form__input_checkbox:checked + label {
  background-color: cadetblue;
  color: #fff;
}

.form__input{
  width: 75%;
}

.form__input_not-valide{
  border-color: red;
}

.form__field{
  display: flex;
  width: 100%;
  margin: 20px 0;
}

.form__error-message{
  align-self: flex-end;
  width: 75%;
  display: block;
  color: red;
  margin: 0;
}

@media(max-width: 600px){
  .form__field  {
    flex-direction: column;
    align-items: flex-start;
  }

  .form__label{
    width: 100%;
  }

  .form__label_radio{
    width: 100%;
    padding: calc(100vw*0.032 - 10px);
  }

  .form__label_checkbox{
    width: 100%;
    margin-left: 0;
    padding: 10px 0;
  }

  .form__input{
    width: 100%;
  }

  .form__error-message{
    width: 100%;
  }

  .form__group_pad{
    flex-direction: column;
    align-items: center;
  }
}

@media(max-width: 1280px){
  .form__group{
    border-radius: calc(100vw*0.047 - 10px);
  }
}

</style>