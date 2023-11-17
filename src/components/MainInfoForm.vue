<template>
  <div class="form-section">
    <h2>Основная информация</h2>

    <div
      class="form-group"
      :class="{
        error: !v$.formData.lastName?.$pending && v$.formData.lastName?.$error,
      }"
    >
      <label for="lastName">Фамилия</label>
      <input
        v-model.trim="localFormData.lastName"
        type="text"
        id="lastName"
        @blur="v$.formData.lastName?.$touch"
      />
      <div
        v-if="!v$.formData.lastName?.$pending && v$.formData.lastName?.$error"
        class="input-errors"
      >
        {{ v$.formData.lastName?.$errors[0]?.$message }}
      </div>
    </div>

    <div
      class="form-group"
      :class="{
        error:
          !v$.formData.firstName?.$pending && v$.formData.firstName?.$error,
      }"
    >
      <label for="firstName">Имя</label>
      <input
        v-model.trim="localFormData.firstName"
        type="text"
        id="firstName"
        @blur="v$.formData.firstName?.$touch"
      />
      <div v-if="!v$.formData.firstName?.$pending" class="input-errors">
        <div
          v-for="error in v$.formData.firstName?.$errors"
          :key="error.$uid"
          class="error-msg"
        >
          {{ error.$message }}
        </div>
      </div>
    </div>

    <div
      class="form-group"
      :class="{
        error:
          !v$.formData.patronymic?.$pending && v$.formData.patronymic?.$error,
      }"
    >
      <label for="patronymic">Отчество</label>
      <input
        v-model.trim="localFormData.patronymic"
        type="text"
        id="patronymic"
        @blur="v$.formData.patronymic?.$touch"
      />
      <div v-if="!v$.formData.patronymic?.$pending" class="input-errors">
        <div
          v-for="error in v$.formData.patronymic?.$errors"
          :key="error.$uid"
          class="error-msg"
        >
          {{ error.$message }}
        </div>
      </div>
    </div>

    <div
      class="form-group"
      :class="{
        error: !v$.formData.dob?.$pending && v$.formData.dob?.$error,
      }"
    >
      <label for="dob">Дата рождения</label>
      <input
        v-model="localFormData.dob"
        type="date"
        id="dob"
        @blur="v$.formData.dob?.$touch"
      />
      <div v-if="!v$.formData.dob?.$pending" class="input-errors">
        <div
          v-for="error in v$.formData.dob?.$errors"
          :key="error.$uid"
          class="error-msg"
        >
          {{ error.$message }}
        </div>
      </div>
    </div>

    <div
      class="form-group"
      :class="{
        error: !v$.formData.phone?.$pending && v$.formData.phone?.$error,
      }"
    >
      <label for="phone">Номер телефона</label>
      <input
        v-model.trim="localFormData.phone"
        type="tel"
        id="phone"
        @input="handlePhoneBlur"
      />
      <div v-if="!v$.formData.phone?.$pending" class="input-errors">
        <div
          v-for="error in v$.formData.phone?.$errors"
          :key="error.$uid"
          class="error-msg"
        >
          {{ error.$message }}
        </div>
      </div>
    </div>

    <div
      class="form-group"
      :class="{
        error: !v$.formData.gender?.$pending && v$.formData.gender?.$error,
      }"
    >
      <label for="gender">Пол</label>
      <CustomSelect
        :value="localFormData.gender"
        id="gender"
        :options="['Мужской', 'Женский']"
        @input="updateFormData('gender', $event)"
      ></CustomSelect>
      <div v-if="!v$.formData.gender?.$pending" class="input-errors">
        <div
          v-for="error in v$.formData.gender?.$errors"
          :key="error.$uid"
          class="error-msg"
        >
          {{ error.$message }}
        </div>
      </div>
    </div>

    <div class="form-section">
      <div
        class="form-group"
        :class="{
          error:
            !v$.formData.clientGroup?.$pending &&
            v$.formData.clientGroup?.$error,
        }"
      >
        <label for="clientGroup">Группа клиентов</label>
        <CustomMultiSelect
          :value="localFormData.clientGroup"
          @input="updateFormData('clientGroup', $event)"
        ></CustomMultiSelect>
        <div v-if="!v$.formData.clientGroup?.$pending" class="input-errors">
          <div
            v-for="error in v$.formData.clientGroup?.$errors"
            :key="error.$uid"
            class="error-msg"
          >
            {{ error.$message }}
          </div>
        </div>
      </div>

      <div
        class="form-group"
        :class="{
          error: !v$.formData.doctor?.$pending && v$.formData.doctor?.$error,
        }"
      >
        <label for="doctor">Лечащий врач</label>
        <CustomSelect
          :value="localFormData.doctor"
          :options="['Иванов', 'Захаров', 'Чернышева']"
          @input="updateFormData('doctor', $event)"
        ></CustomSelect>
        <div v-if="!v$.formData.doctor?.$pending" class="input-errors">
          <div
            v-for="error in v$.formData.doctor?.$errors"
            :key="error.$uid"
            class="error-msg"
          >
            {{ error.$message }}
          </div>
        </div>
      </div>

      <div class="form-group">
        <label>
          <input v-model="localFormData.noSMS" type="checkbox" />
          Не отправлять СМС
        </label>
      </div>
    </div>
  </div>
</template>

<script>
import CustomMultiSelect from "./CustomMultiSelect.vue";
import CustomSelect from "./CustomSelect.vue";

export default {
  components: {
    CustomMultiSelect,
    CustomSelect,
  },
  props: {
    formData: Object,
    v$: Object,
  },
  computed: {
    localFormData: {
      get() {
        return this.formData;
      },
      set(value) {
        this.updateFormData("localFormData", value);
      },
    },
  },
  methods: {
    updateFormData(field, value) {
      this.$emit("updateFormData", { field, value });
    },
    handlePhoneBlur() {
      // Приводим введенный телефон к нужному формату, например, "+7 (XXX) XXX-XXXX"
      const rawPhoneNumber = this.localFormData.phone.replace(/\D/g, "");
      const formattedPhoneNumber = `+7 (${rawPhoneNumber.slice(
        1,
        4
      )}) ${rawPhoneNumber.slice(4, 7)}-${rawPhoneNumber.slice(7, 11)}`;

      // Обновляем значение в модели
      this.localFormData.phone = formattedPhoneNumber;
    },
  },
};
</script>

<style scoped lang="scss">
.form-section {
  margin-bottom: 20px;

  h2 {
    font-size: 18px;
    margin-bottom: 10px;
  }

  .form-group {
    margin-bottom: 15px;

    label {
      display: block;
      margin-bottom: 5px;
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
    }

    .input-errors {
      color: red;
      font-size: 12px;
      margin-top: 5px;
    }

    .error input,
    .error select {
      border-color: red;
    }

    .error-msg {
      color: red;
      font-size: 12px;
    }

    .error-border {
      border: 1px solid red;
    }
  }

  label[for="lastName"]:after,
  label[for="firstName"]:after,
  label[for="dob"]:after,
  label[for="phone"]:after,
  label[for="clientGroup"]:after {
    content: "*";
    color: red;
    margin-left: 2px;
  }

  .form-section {
    margin-bottom: 20px;
  }

  .form-group {
    margin-bottom: 15px;
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
  }

  button {
    background-color: #4caf50;
    color: #fff;
    padding: 10px 15px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }

  button:hover {
    background-color: #45a049;
  }

  .input-errors {
    color: red;
    font-size: 12px;
    margin-top: 5px;
  }

  .error input,
  .error select {
    border-color: red;
  }

  .error-msg {
    color: red;
    font-size: 12px;
  }

  .error-border {
    border: 1px solid red;
  }

  @media (max-width: 768px) {
    .form-section {
      margin-bottom: 10px;
    }

    .form-group {
      margin-bottom: 10px;
    }

    input,
    select {
      margin-bottom: 5px;
    }

    button {
      padding: 8px 12px;
    }
  }
}
</style>
