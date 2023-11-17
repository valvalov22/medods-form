<template>
  <div class="form-container">
    <h1>Форма создания клиента</h1>
    <form @submit.prevent="handleSubmit" class="client-form">
      <!-- Основная информация о клиенте -->
      <div v-if="showSuccessMessage" class="success-message">
        Клиент успешно создан
      </div>
      <MainInfoForm
        :form-data="formData"
        :v$="v$"
        @updateFormData="updateFormData"
      >
      </MainInfoForm>

      <!-- Адрес -->
      <AddressForm
        :form-data="formData"
        :v$="v$"
        @updateFormData="updateFormData"
      >
      </AddressForm>

      <!-- Документы -->
      <PassportForm
        :form-data="formData"
        :v$="v$"
        @updateFormData="updateFormData"
      ></PassportForm>

      <button type="submit" :disabled="v$.$pending">Создать клиента</button>
    </form>
  </div>
</template>

<script>
import { useVuelidate } from "@vuelidate/core";
import {
  required,
  numeric,
  helpers,
  minLength,
  maxLength,
} from "@vuelidate/validators";

import MainInfoForm from "@/components/MainInfoForm.vue";
import AddressForm from "./AddressForm.vue";
import PassportForm from "./PassportForm.vue";

const isAlpha = helpers.regex(/^[a-zA-Zа-яА-Я]+$/);

const isAlphaMessage = "Используйте буквы русского или английского алфавита";
const requiredMessage = "Обязательное поле";
const numericMessage = "Используйте цифры";
const maxLengthMessage = "Максимальная длина равна 30 символов";
const phoneLength = "Введите корректный номер";

export default {
  components: {
    MainInfoForm,
    AddressForm,
    PassportForm,
  },
  setup() {
    const v$ = useVuelidate();
    return { v$ };
  },
  data() {
    return {
      showSuccessMessage: false,
      formData: {
        lastName: "",
        firstName: "",
        patronymic: "",
        dob: "",
        phone: "",
        gender: "Мужской",
        clientGroup: [],
        doctor: "Выберите лечащего врача",
        noSMS: false,
        address: {
          index: "",
          country: "",
          region: "",
          city: "",
          street: "",
          house: "",
        },
        passport: {
          docType: "",
          series: "",
          number: "",
          issuedBy: "",
          issuedDate: "",
        },
      },
    };
  },
  validations() {
    return {
      formData: {
        lastName: {
          required: helpers.withMessage(requiredMessage, required),
          isAlpha: helpers.withMessage(isAlphaMessage, isAlpha),
          maxLength: helpers.withMessage(maxLengthMessage, maxLength(30)),
        },
        firstName: {
          required: helpers.withMessage(requiredMessage, required),
          isAlpha: helpers.withMessage(isAlphaMessage, isAlpha),
          maxLength: helpers.withMessage(maxLengthMessage, maxLength(30)),
        },
        patronymic: {
          isAlpha: helpers.withMessage(isAlphaMessage, isAlpha),
          maxLength: helpers.withMessage(maxLengthMessage, maxLength(30)),
        },
        dob: { required: helpers.withMessage(requiredMessage, required) },
        phone: {
          required: helpers.withMessage(requiredMessage, required),
          minLength: helpers.withMessage(phoneLength, minLength(17)),
        },
        clientGroup: {
          required: helpers.withMessage(requiredMessage, required),
        },
        address: {
          index: { numeric: helpers.withMessage(numericMessage, numeric) },
          country: { isAlpha: helpers.withMessage(isAlphaMessage, isAlpha) },
          region: { isAlpha: helpers.withMessage(isAlphaMessage, isAlpha) },
          city: {
            required: helpers.withMessage(requiredMessage, required),
            isAlpha: helpers.withMessage(isAlphaMessage, isAlpha),
          },
          house: {
            numeric: helpers.withMessage(numericMessage, numeric),
          },
        },
        passport: {
          docType: {
            required: helpers.withMessage(requiredMessage, required),
          },
          series: { numeric: helpers.withMessage(numericMessage, numeric) },
          number: { numeric: helpers.withMessage(numericMessage, numeric) },
          issuedDate: {
            required: helpers.withMessage(requiredMessage, required),
          },
        },
      },
    };
  },
  methods: {
    handleSubmit() {
      this.v$.$touch();

      if (this.v$.$pending) {
        console.log("Форма в процессе валидации. Пожалуйста, подождите.");
        return;
      }

      if (this.v$.$error) {
        console.log("Форма невалидна");
        return;
      }

      console.log("Данные формы:", this.formData);

      this.formData = {
        lastName: "",
        firstName: "",
        patronymic: "",
        dob: "",
        phone: "",
        gender: "Мужской",
        clientGroup: [],
        doctor: "Выберите лечащего врача",
        noSMS: false,
        address: {
          index: "",
          country: "",
          region: "",
          city: "",
          street: "",
          house: "",
        },
        passport: {
          docType: "",
          series: "",
          number: "",
          issuedBy: "",
          issuedDate: "",
        },
      };

      // Отмечаем форму как не тронутую
      this.v$.$reset();

      this.showSuccessMessage = true;
      let countdown = 5;
      const countdownInterval = setInterval(() => {
        countdown--;

        // Обновляем значение обратного отсчета
        this.countdownValue = countdown;

        if (countdown <= 0) {
          // Скрываем сообщение об успехе и останавливаем интервал
          this.showSuccessMessage = false;
          clearInterval(countdownInterval);
        }
      }, 1000);
    },
    updateFormData({ field, value }) {
      this.formData = { ...this.formData, [field]: value };
    },
  },
};
</script>

<style scoped lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap");

.success-message {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: #4caf50;
  color: white;
  padding: 20px;
  border-radius: 4px;
  text-align: center;
  font-size: 18px;
  z-index: 100;
}

.countdown {
  margin-top: 10px;
  font-size: 16px;
}

.form-container {
  width: 600px;
  margin: 0 auto;
  padding: 20px;
  font-family: "Roboto", sans-serif;

  @media (max-width: 768px) {
    width: 100%;
  }
}

.client-form {
  background-color: #f4f4f4;
  color: #000;
  padding: 20px;
  border-radius: 8px;

  @media (max-width: 768px) {
    padding: 10px;
  }
}

.form-group {
  margin-bottom: 15px;

  @media (max-width: 768px) {
    margin-bottom: 10px;
  }
}

input,
select {
  width: 100%;
  padding: 8px;
  box-sizing: border-box;
  margin-top: 5px;
  margin-bottom: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-family: "Roboto", sans-serif;

  @media (max-width: 768px) {
    margin-bottom: 5px;
  }
}

button {
  background-color: #ffc107; /* Желтый цвет */
  color: #000; /* Черный цвет */
  padding: 10px 15px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-family: "Roboto", sans-serif;

  @media (max-width: 768px) {
    padding: 8px 12px;
  }
}

button:hover {
  background-color: #ff9800; /* Замена цвета при наведении */
}
</style>
