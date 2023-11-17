<template>
  <div class="custom-multi-select">
    <div class="select-label" @click="toggleDropdown">
      <div
        v-if="selectedOptions.length > 0"
        class="selected-options"
        @click.stop
      >
        <div
          v-for="option in selectedOptions"
          :key="option"
          class="selected-option"
        >
          {{ option }}
          <span @click="removeOption(option)">x</span>
        </div>
      </div>
      <div v-else>Группа клиентов</div>
    </div>

    <div class="select-container" v-show="isDropdownOpen">
      <div class="dropdown">
        <div
          v-for="item in availableOptions"
          :key="item"
          :class="{ selected: selectedOptions.includes(item) }"
          @click="toggleOption(item)"
        >
          {{ item }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    value: Array,
  },
  data() {
    return {
      availableOptions: ["VIP", "Проблемные", "ОМС"],
      selectedOptions: this.value || [],
      isDropdownOpen: false,
    };
  },
  methods: {
    toggleOption(item) {
      if (this.selectedOptions.includes(item)) {
        this.removeOption(item);
      } else {
        this.selectedOptions.push(item);
      }
      this.$emit("input", this.selectedOptions);
    },
    removeOption(option) {
      this.selectedOptions = this.selectedOptions.filter(
        (item) => item !== option
      );
      this.$emit("input", this.selectedOptions);
    },
    toggleDropdown() {
      this.isDropdownOpen = !this.isDropdownOpen;
    },
  },
};
</script>

<style scoped lang="scss">
.custom-multi-select {
  width: 100%;
  margin-bottom: 20px;

  .select-container {
    position: relative;
  }

  .select-label {
    cursor: pointer;
    background-color: #ffc107; /* Желтый цвет */
    color: #000; /* Черный цвет текста */
    padding: 10px;
    border-radius: 4px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    height: 40px; /* Фиксированная высота */
    overflow: hidden; /* Скрыть содержимое, выходящее за пределы фиксированной высоты */
  }

  .selected-options {
    display: flex;
    flex-wrap: wrap;
    margin-top: 5px;
  }

  .selected-option {
    background-color: #4caf50; /* Зеленый цвет */
    color: #fff; /* Белый цвет текста */
    padding: 8px;
    border-radius: 4px;
    margin: 2px;
    display: flex;
    align-items: center;
    cursor: pointer;

    span {
      margin-left: 5px;
      cursor: pointer;
    }
  }

  .dropdown {
    z-index: 10000;
    position: absolute;
    top: 100%;
    left: 0;
    width: 100%;
    background-color: #fff;
    border: 1px solid #ffc107; /* Желтый цвет */
    border-top: none;
    border-radius: 0 0 4px 4px;

    div {
      padding: 8px;
      cursor: pointer;

      &:hover {
        background-color: #e0e0e0;
      }

      &.selected {
        position: relative;
        padding-left: 20px;

        &:before {
          content: "✓";
          position: absolute;
          left: 5px;
        }
      }
    }
  }
}
</style>
