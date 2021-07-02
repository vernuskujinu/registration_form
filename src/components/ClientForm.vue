<template>
  <div class="container">
    <form
      name="client-form"
      class="client-form flex-direction-column mx-auto"
      @submit.prevent="submit"
    >
      <div class="form-content">
        <h1>Форма регистрации</h1>
        <!-- Основная иформация о клиенте-->
        <div class="basic-info">
          <div class="input-field" :class="{ errorInput: $v.clientSurname.$error }">
            <label for="surname">Фамилия*</label>
            <input type="text" v-model="clientSurname" :class="{ error: $v.clientSurname.$error }" />
            <div class="error" v-if="!$v.clientSurname.maxLength">Слишком длинная фамилия</div>
            <div class="error" v-if="!$v.clientSurname.required">Введите фамилию</div>
            <div class="error" v-if="!$v.clientSurname.nameValidation">Некорректное значение</div>
          </div>

          <div class="input-field" :class="{ errorInput: $v.clientName.$error }">
            <label for="name">Имя*</label>
            <input type="text" v-model="clientName" :class="{ error: $v.clientName.$error }" />
            <div class="error" v-if="!$v.clientName.maxLength">Слишком длинное имя</div>
            <div class="error" v-if="!$v.clientName.required">Введите имя</div>
            <div class="error" v-if="!$v.clientName.nameValidation">Некорректное значение</div>
          </div>

          <div class="input-field" :class="{ errorInput: $v.clientPatronymic.$error }">
            <label for="patronymic">Отчество</label>
            <input
              type="text"
              v-model="clientPatronymic"
              :class="{ error: $v.clientPatronymic.$error }"
            />
            <div class="error" v-if="!$v.clientPatronymic.maxLength">Слишком длинное отчество</div>
            <div class="error" v-if="!$v.clientPatronymic.nameValidation">Некорректное значение</div>
          </div>

          <div class="input-field" :class="{ errorInput: $v.birthday.$error }">
            <label for="birthday">Дата рождения*</label>
            <input type="date" v-model="birthday" :class="{ error: $v.birthday.$error }" />
            <div class="error" v-if="!$v.birthday.required">Введите дату рождения</div>
          </div>

          <div class="input-field" :class="{ errorInput: $v.phone.$error }">
            <label for="tel">Телефон</label>
            <input
              type="tel"
              placeholder="79222222222"
              v-model="phone"
              :class="{ error: $v.phone.$error }"
            />
            <div class="error" v-if="!$v.phone.isPhoneNumber">Введите телефон в формате: 79222222222</div>
            <div class="error" v-if="!$v.phone.maxLength">Максимум 11 цифр</div>
          </div>

          <div class="input-field">
            <label>Выберите пол</label>
            <select v-model="gender">
              <option value="male">Мужской</option>
              <option value="famale">Женский</option>
            </select>
          </div>

          <div class="input-field" :class="{ errorInput: $v.clientGroup.$error }">
            <label>Выберите группу клиента*</label>
            <select v-model="clientGroup" :class="{ error: $v.clientGroup.$error }">
              <option value="VIP">VIP</option>
              <option value="hard-clients">Проблемные</option>
              <option value="OMS">ОМС</option>
            </select>
            <div class="error" v-if="!$v.clientGroup.required">Выберите группу клиента</div>
          </div>

          <div class="input-field">
            <label>Выберите врача</label>
            <select name="doctor" v-model="doctor">
              <option value="ivanov">Иванов</option>
              <option value="petrov">Захаров</option>
              <option value="sidorov">Чернышева</option>
            </select>
          </div>
        </div>
        <!-- Адрес клиента-->
        <div class="adress-container">
          <h2>Адрес</h2>
          <div class="input-field" :class="{ errorInput: $v.postIndex.$error }">
            <label for="post-index">Индекс</label>
            <input type="text" v-model="postIndex" :class="{ error: $v.postIndex.$error }" />
            <div class="error" v-if="!$v.city.numeric">Введите числовое значение</div>
            <div class="error" v-if="!$v.city.maxLength">Слишком длинное значние</div>
          </div>

          <div class="input-field" :class="{ errorInput: $v.country.$error }">
            <label for="country">Страна</label>
            <input type="country" v-model="country" :class="{ error: $v.country.$error }" />
            <div class="error" v-if="!$v.country.nameValidation">Введите буквенное значение</div>
            <div class="error" v-if="!$v.country.maxLength">Слишком длинное название</div>
          </div>

          <div class="input-field" :class="{ errorInput: $v.region.$error }">
            <label for="region">Область</label>
            <input type="text" v-model="region" :class="{ error: $v.region.$error }" />
            <div class="error" v-if="!$v.region.nameValidation">Введите буквенное значение</div>
            <div class="error" v-if="!$v.region.maxLength">Слишком длинное название</div>
          </div>

          <div class="input-field" :class="{ errorInput: $v.city.$error }">
            <label for="city">Город*</label>
            <input type="text" v-model="city" :class="{ error: $v.city.$error }" />
            <div class="error" v-if="!$v.city.required">Введите название населенного пункта</div>
            <div class="error" v-if="!$v.city.nameValidation">Введите буквенное значение</div>
            <div class="error" v-if="!$v.city.maxLength">Слишком длинное название</div>
          </div>

          <div class="input-field" :class="{ errorInput: $v.street.$error }">
            <label for="street">Улица</label>
            <input type="text" v-model="street" :class="{ error: $v.street.$error }" />
            <div class="error" v-if="!$v.street.nameValidation">Введите буквенное значение</div>
            <div class="error" v-if="!$v.street.maxLength">Слишком длинное название</div>
          </div>

          <div class="input-field" :class="{ errorInput: $v.building.$error }">
            <label for="building">Дом</label>
            <input type="text" v-model="building" :class="{ error: $v.building.$error }" />
            <div class="error" v-if="!$v.building.numeric">Введите числовое значение</div>
            <div class="error" v-if="!$v.building.maxLength">Слишком длинное значние</div>
          </div>
        </div>
        <!-- Документ клиента-->
        <div class="personal-document-container">
          <h2>Документ</h2>
          <div class="input-field" :class="{ errorInput: $v.PersonalDocument.$error }">
            <label>Выберите тип документа*</label>
            <select
              name="personal-document"
              v-model="PersonalDocument"
              :class="{ error: $v.PersonalDocument.$error }"
            >
              <option value="pasport">Паспорт</option>
              <option value="certificate">Свидетельство о рождении</option>
              <option value="license">Водительские права</option>
            </select>
            <div class="error" v-if="!$v.PersonalDocument.required">Необходимо выбрать тип документа</div>
          </div>

          <div class="input-field" :class="{ errorInput: $v.documentSeries.$error }">
            <label for="series">Серия</label>
            <input
              type="text"
              v-model="documentSeries"
              :class="{ error: $v.documentSeries.$error }"
            />
            <div class="error" v-if="!$v.documentSeries.numeric">Введите числовое значение</div>
            <div class="error" v-if="!$v.documentSeries.maxLength">Слишком длинное значние</div>
          </div>

          <div class="input-field" :class="{ errorInput: $v.documentNumber.$error }">
            <label for="number">Номер</label>
            <input
              type="text"
              v-model="documentNumber"
              :class="{ error: $v.documentNumber.$error }"
            />
            <div class="error" v-if="!$v.documentNumber.numeric">Введите числовое значение</div>
            <div class="error" v-if="!$v.documentNumber.maxLength">Слишком длинное значние</div>
          </div>

          <div class="input-field" :class="{ errorInput: $v.issued.$error }">
            <label for="issued">Кем выдан</label>
            <input type="text" v-model="issued" :class="{ error: $v.issued.$error }" />
            <div class="error" v-if="!$v.issued.nameValidation">Введите буквенное значение</div>
            <div class="error" v-if="!$v.issued.maxLength">Слишком длинное название</div>
          </div>

          <div class="input-field" :class="{ errorInput: $v.dateOfIssue.$error }">
            <label for="date-of-issue">Дата выдачи*</label>
            <input type="date" v-model="dateOfIssue" :class="{ error: $v.dateOfIssue.$error }" />
            <div class="error" v-if="!$v.dateOfIssue.required">Введите дату выдачи</div>
          </div>
          <span class="footnote">* - поля, обязательные для заполнения</span>
          <div class="checkbox-field">
            <label for="sms">Не отправлять SMS</label>
            <input type="checkbox" v-model="smsSend" />
          </div>
        </div>
      </div>
      <div class="form-action">
        <button type="submit">Зарегистрироваться</button>
      </div>
    </form>
  </div>
</template>

<script>
import {
  required,
  maxLength,
  numeric,
  helpers,
} from "vuelidate/lib/validators";
import "../sass/style.sass";

const nameValidation = helpers.regex("nameValidation", /^[a-zA-ZАа-яёА-я ]*$/);
const isPhoneNumber = helpers.regex("isPhoneNumber", /(8|\+7|7)[0-9]{7,10}/);

export default {
  name: "ClientForm",
  data() {
    return {
      clientSurname: "",
      clientName: "",
      clientPatronymic: "",
      birthday: "",
      phone: "",
      gender: "",
      clientGroup: "",
      doctor: "",
      postIndex: "",
      country: "",
      region: "",
      city: "",
      street: "",
      building: "",
      PersonalDocument: "",
      documentSeries: "",
      documentNumber: "",
      issued: "",
      dateOfIssue: "",
      smsSend: "",
    };
  },
  validations: {
    clientSurname: {
      required,
      nameValidation,
      maxLength: maxLength(25),
    },
    clientName: {
      required,
      nameValidation,
      maxLength: maxLength(25),
    },
    clientPatronymic: {
      nameValidation,
      maxLength: maxLength(25),
    },
    birthday: {
      required,
    },
    phone: {
      isPhoneNumber,
      maxLength: maxLength(11),
    },
    clientGroup: {
      required,
    },
    postIndex: {
      numeric,
      maxLength: maxLength(6),
    },
    country: {
      nameValidation,
      maxLength: maxLength(25),
    },
    region: {
      nameValidation,
      maxLength: maxLength(25),
    },
    city: {
      required,
      nameValidation,
      maxLength: maxLength(25),
    },
    street: {
      nameValidation,
      maxLength: maxLength(25),
    },
    building: {
      numeric,
      maxLength: maxLength(4),
    },
    PersonalDocument: {
      required,
    },
    documentSeries: {
      numeric,
      maxLength: maxLength(12),
    },
    documentNumber: {
      numeric,
      maxLength: maxLength(25),
    },
    issued: {
      nameValidation,
      maxLength: maxLength(150),
    },
    dateOfIssue: {
      required,
    },
  },
  methods: {
    submit(event) {
      this.$v.$touch();
      if (this.$v.$invalid) {
        this.submitStatus = "ERROR";
      } else {
        // do your submit logic here
        let clientObj = {
          clientSurname: this.clientSurname,
          clientName: this.clientName,
          clientPatronymic: this.clientPatronymic,
          birthday: this.birthday,
          phone: this.phone,
          gender: this.gender,
          clientGroup: this.clientGroup,
          doctor: this.doctor,
          postIndex: this.postIndex,
          country: this.country,
          region: this.region,
          city: this.city,
          street: this.street,
          building: this.building,
          PersonalDocument: this.PersonalDocument,
          documentSeries: this.documentSeries,
          documentNumber: this.documentNumber,
          issued: this.issued,
          dateOfIssue: this.dateOfIssue,
          smsSend: this.smsSend,
        };
        console.log(clientObj);
        alert("Клиент успешно создан");
        event.target.reset();
        this.submitStatus = "PENDING";
        setTimeout(() => {
          this.submitStatus = "OK";
        }, 500);
        return clientObj;
      }
    },
  },
};
</script>

<style scoped lang="sass"></style>
