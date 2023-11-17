<template>
  <div class="form-section">
    <h2>Адрес</h2>

    <div
      class="form-group"
      :class="{
        error:
          !v$.formData.address?.index?.$pending &&
          v$.formData.address?.index?.$error,
      }"
    >
      <label for="index">Индекс</label>
      <input
        v-model.trim="localFormData.address.index"
        type="text"
        id="index"
        @blur="v$.formData.address?.index?.$touch"
      />
      <div v-if="!v$.formData.address?.index?.$pending" class="input-errors">
        <div
          v-for="error in v$.formData.address?.index?.$errors"
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
          !v$.formData.address?.country?.$pending &&
          v$.formData.address?.country?.$error,
      }"
    >
      <label for="country">Страна</label>
      <input
        v-model.trim="localFormData.address.country"
        type="text"
        id="country"
        @blur="v$.formData.address?.country?.$touch"
      />
      <div v-if="!v$.formData.address?.country?.$pending" class="input-errors">
        <div
          v-for="error in v$.formData.address?.country?.$errors"
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
          !v$.formData.address?.region?.$pending &&
          v$.formData.address?.region?.$error,
      }"
    >
      <label for="region">Область</label>
      <input
        v-model.trim="localFormData.address.region"
        type="text"
        id="region"
        @blur="v$.formData.address?.region?.$touch"
      />
      <div v-if="!v$.formData.address?.region?.$pending" class="input-errors">
        <div
          v-for="error in v$.formData.address?.region?.$errors"
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
          !v$.formData.address?.city?.$pending &&
          v$.formData.address?.city?.$error,
        'error-border': v$.formData.address?.city?.$pending,
      }"
    >
      <label for="city">Город</label>
      <input
        v-model.trim="localFormData.address.city"
        type="text"
        id="city"
        @blur="v$.formData.address?.city?.$touch"
      />
      <div
        v-if="
          !v$.formData.address?.city?.$pending &&
          v$.formData.address?.city?.$error &&
          v$.formData.address?.city?.$errors.length > 0
        "
        class="input-errors"
      >
        <div
          v-for="error in v$.formData.address?.city?.$errors"
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
          !v$.formData.address?.street?.$pending &&
          v$.formData.address?.street?.$error,
      }"
    >
      <label for="street">Улица</label>
      <input
        v-model.trim="localFormData.address.street"
        type="text"
        id="street"
        @blur="v$.formData.address?.street?.$touch"
      />
      <div v-if="!v$.formData.address?.street?.$pending" class="input-errors">
        <div
          v-for="error in v$.formData.address?.street?.$errors"
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
          !v$.formData.address?.house?.$pending &&
          v$.formData.address?.house?.$error,
      }"
    >
      <label for="house">Дом</label>
      <input
        v-model.trim="localFormData.address.house"
        type="text"
        id="house"
        @blur="v$.formData.address?.house?.$touch"
      />
      <div v-if="!v$.formData.address?.house?.$pending" class="input-errors">
        <div
          v-for="error in v$.formData.address?.house?.$errors"
          :key="error.$uid"
          class="error-msg"
        >
          {{ error.$message }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
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
        this.$emit("updateFormData", { field: "address", value });
      },
    },
  },
  methods: {
    updateFormData(field, value) {
      this.localFormData = { ...this.localFormData, [field]: value };
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

  label[for="city"]:after {
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
