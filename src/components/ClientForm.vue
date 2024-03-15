<template>
  <form @submit.prevent="submitForm" id="form">
    <div class="form_content">
      <div>
        <h2>Личная информация</h2>

        <div class="data">
          <label>Фамилия:*</label>
          <span
            v-if="
              formData.formSubmitted &&
              !$v.formData.personalInfo.lastName.required
            "
            >Поле обязательно</span
          >
          <input
            type="text"
            id="lastName"
            v-model="formData.personalInfo.lastName"
            placeholder="Иванов"
          />
        </div>

        <div class="data">
          <label>Имя:*</label>
          <span
            v-if="
              formData.formSubmitted && !$v.formData.personalInfo.name.required
            "
            >Поле обязательно</span
          >
          <input
            type="text"
            id="name"
            v-model="formData.personalInfo.name"
            placeholder="Иван"
          />
        </div>

        <div class="data">
          <label>Отчество:</label>
          <input
            type="text"
            v-model="formData.personalInfo.middleName"
            placeholder="Иванович"
          />
        </div>

        <div class="data">
          <label>Дата рождения:*</label>
          <span
            v-if="
              formData.formSubmitted &&
              !$v.formData.personalInfo.birthDate.required
            "
            >Поле обязательно</span
          >
          <input
            type="date"
            id="birthDate"
            v-model="formData.personalInfo.birthDate"
          />
        </div>

        <div class="data">
          <label>Номер телефона:*</label>
          <span
            v-if="
              formData.formSubmitted &&
              (!$v.formData.personalInfo.phone.required ||
                ($v.formData.personalInfo.phone.required &&
                  formData.personalInfo.phone === '7'))
            "
            >Поле обязательно</span
          >
          <span v-else-if="!$v.formData.personalInfo.phone.numeric"
            >Пожалуйста, введите только цифры</span
          >
          <span
            v-else-if="
              formData.isNumeric && formData.personalInfo.phone.length !== 11
            "
            >Введите 11 символов</span
          >
          <input
            type="text"
            id="phone"
            v-model="formData.personalInfo.phone"
            @keydown="preventDelete"
            @input="checkNumeric"
          />
        </div>

        <div class="gender">
          <label>Пол:</label>
          <div>
            <input
              type="radio"
              id="male"
              value="male"
              v-model="formData.personalInfo.gender"
            />
            <label>Мужчина</label>
          </div>
          <div>
            <input
              type="radio"
              id="female"
              value="female"
              v-model="formData.personalInfo.gender"
            />
            <label>Женщина</label>
          </div>
        </div>

        <div class="data">
          <label>Группа клиентов:*</label>
          <span
            v-if="
              formData.formSubmitted &&
              !$v.formData.personalInfo.clientGroup.required
            "
            >Поле обязательно</span
          >
          <select multiple v-model="formData.personalInfo.clientGroup">
            <option value="VIP">VIP</option>
            <option value="Проблемные">Проблемные</option>
            <option value="ОМС">ОМС</option>
          </select>
        </div>

        <div class="data">
          <label>Лечащий врач: </label>
          <select v-model="formData.personalInfo.attendingDoctor">
            <option value="Иванов">Иванов</option>
            <option value="Захаров">Захаров</option>
            <option value="Чернышева">Чернышева</option>
          </select>
        </div>

        <label>Не отправлять СМС </label
        ><input type="checkbox" v-model="formData.personalInfo.noSms" />
      </div>

      <div>
        <h2>Адрес</h2>
        <div class="data">
          <label>Индекс:</label>
          <input
            type="text"
            v-model="formData.address.zip"
            placeholder="142000"
          />
        </div>
        <div class="data">
          <label>Страна:</label>
          <input
            type="text"
            v-model="formData.address.country"
            placeholder="Россия"
          />
        </div>

        <div class="data">
          <label>Область:</label
          ><input
            type="text"
            v-model="formData.address.region"
            placeholder="Московская область"
          />
        </div>

        <div class="data">
          <label>Город:*</label
          ><span
            v-if="formData.formSubmitted && !$v.formData.address.city.required"
            >Поле обязательно</span
          >
          <input
            type="text"
            v-model="formData.address.city"
            placeholder="Домодедово"
          />
        </div>

        <div class="data">
          <label>Улица:</label>
          <input
            type="text"
            v-model="formData.address.street"
            placeholder="Южная"
          />
        </div>

        <div class="data">
          <label>Дом:</label>
          <input
            type="text"
            v-model="formData.address.house"
            placeholder="10"
          />
        </div>
      </div>

      <div>
        <h2>Паспорт</h2>
        <div class="data">
          <label>Тип документа:*</label>
          <span
            v-if="formData.formSubmitted && !$v.formData.passport.type.required"
            >Поле обязательно</span
          >
          <select v-model="formData.passport.type">
            <option value="Паспорт">Паспорт</option>
            <option value="Свидетельство о рождении">
              Свидетельство о рождении
            </option>
            <option value="Вод. удостоверение">Вод. удостоверение</option>
          </select>
        </div>

        <div class="data">
          <label>Серия:</label>
          <input
            type="text"
            v-model="formData.passport.series"
            placeholder="1234"
          />
        </div>

        <div class="data">
          <label>Номер:</label
          ><input
            type="text"
            v-model="formData.passport.number"
            placeholder="123456"
          />
        </div>
        <div class="data">
          <label>Кем выдан:</label>
          <input
            type="text"
            v-model="formData.passport.issuedBy"
            placeholder="ГУВМ МВД"
          />
        </div>
        <div class="data">
          <label>Дата выдачи*:</label>
          <span
            v-if="
              formData.formSubmitted && !$v.formData.passport.issueDate.required
            "
            >Поле обязательно</span
          >
          <input type="date" v-model="formData.passport.issueDate" />
        </div>
      </div>
    </div>
    <button type="submit">Отправить</button>
  </form>
</template>

<script>
import { required, numeric } from "vuelidate/lib/validators";

export default {
  name: "ClientForm",
  data() {
    return {
      formData: {
        personalInfo: {
          lastName: "",
          name: "",
          middleName: "",
          birthDate: "",
          phone: "7",
          gender: "",
          clientGroup: [],
          attendingDoctor: "",
          noSms: false,
        },
        address: {
          zip: "",
          country: "",
          region: "",
          city: "",
          street: "",
          house: "",
        },
        passport: {
          type: "",
          series: "",
          number: "",
          issuedBy: "",
          issueDate: "",
        },
        formSubmitted: false,
        isNumeric: false,
      },
    };
  },
  validations: {
    formData: {
      personalInfo: {
        lastName: {
          required,
        },
        name: {
          required,
        },
        birthDate: {
          required,
        },
        phone: {
          required,
          numeric,
        },
        clientGroup: {
          required,
        },
      },
      address: {
        city: {
          required,
        },
      },
      passport: {
        type: {
          required,
        },
        issueDate: {
          required,
        },
      },
    },
  },
  methods: {
    preventDelete(event) {
      if (
        event.key === "Backspace" &&
        this.formData.personalInfo.phone === "7"
      ) {
        event.preventDefault();
      }
    },
    checkNumeric() {
      // Проверяю, содержит ли введённая строка только цифры
      this.formData.isNumeric = /^\d+$/.test(this.formData.personalInfo.phone);
    },
    submitForm() {
      this.$v.$touch();

      // Проверяю, есть ли ошибки валидации
      if (!this.$v.$error) {
        alert("Успешно, новый клиент создан :)");
        //  this.formData.formSubmitted = true;
      } else {
        // Если есть ошибки, отмечаю, что форма отправлена, чтобы показать сообщения об ошибках
        this.formData.formSubmitted = true;
      }
    },
  },
};
</script>

<style lang="sass">
#form
  display: flex
  flex-direction: column
  justify-content: space-around
  border: 1px solid #000
  border-radius: 20px
  background-color: #e6e0e0
  margin: 10px auto
  padding: 15px
  width: 100%
  gap: 10px

  .form_content
   display: flex
   flex-direction: row
   align-items: flex-start
   justify-content: space-around
   gap: 10px
   flex-wrap: wrap

.data
 display: flex
 flex-direction: column
 gap: 3px
 margin-bottom: 8px
 max-width: 300px
.gender
 display: flex
 margin-bottom: 8px
</style>
