<template>
  <div class="form-section">
    <h2>Документы</h2>

    <div
      class="form-group"
      :class="{
        error:
          !v$.formData.passport?.docType?.$pending &&
          v$.formData.passport?.docType?.$error,
        'error-border': v$.formData.passport?.docType?.$pending,
      }"
    >
      <label for="docType">Тип документа</label>
      <CustomSelect
        v-model="localFormData.passport.docType"
        :options="[
          'Паспорт',
          'Свидетельство о рождении',
          'Водительское удостоверение',
        ]"
      ></CustomSelect>
      <div v-if="!v$.formData.passport?.docType?.$pending" class="input-errors">
        <div
          v-for="error in v$.formData.passport?.docType?.$errors"
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
          !v$.formData.passport?.series?.$pending &&
          v$.formData.passport?.series?.$error,
      }"
    >
      <label for="series">Серия</label>
      <input
        v-model.trim="localFormData.passport.series"
        type="text"
        id="series"
        @blur="v$.formData.passport?.series?.$touch"
      />
      <div v-if="!v$.formData.passport?.series?.$pending" class="input-errors">
        <div
          v-for="error in v$.formData.passport?.series?.$errors"
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
          !v$.formData.passport?.number?.$pending &&
          v$.formData.passport?.number?.$error,
      }"
    >
      <label for="number">Номер</label>
      <input
        v-model.trim="localFormData.passport.number"
        type="text"
        id="number"
        @blur="v$.formData.passport?.number?.$touch"
      />
      <div v-if="!v$.formData.passport?.number?.$pending" class="input-errors">
        <div
          v-for="error in v$.formData.passport?.number?.$errors"
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
          !v$.formData.passport?.issuedBy?.$pending &&
          v$.formData.passport?.issuedBy?.$error,
      }"
    >
      <label for="issuedBy">Кем выдан</label>
      <input
        v-model.trim="localFormData.passport.issuedBy"
        type="text"
        id="issuedBy"
        @blur="v$.formData.passport?.issuedBy?.$touch"
      />
      <div
        v-if="!v$.formData.passport?.issuedBy?.$pending"
        class="input-errors"
      >
        <div
          v-for="error in v$.formData.passport?.issuedBy?.$errors"
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
          !v$.formData.passport?.issuedDate?.$pending &&
          v$.formData.passport?.issuedDate?.$error,
      }"
    >
      <label for="issuedDate">Дата выдачи</label>
      <input
        v-model="localFormData.passport.issuedDate"
        type="date"
        id="issuedDate"
        @blur="v$.formData.passport?.issuedDate?.$touch"
      />
      <div
        v-if="!v$.formData.passport?.issuedDate?.$pending"
        class="input-errors"
      >
        <div
          v-for="error in v$.formData.passport?.issuedDate?.$errors"
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
import CustomSelect from "./CustomSelect.vue";

export default {
  components: {
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
        this.$emit("updateFormData", { field: "passport", value });
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

  label[for="docType"]:after,
  label[for="issuedDate"]:after {
    content: "*";
    color: red;
    margin-left: 2px;
  }

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

    input {
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

    .error input {
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
}
</style>
