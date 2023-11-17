<template>
  <div class="custom-select">
    <div class="select-label" @click="toggleDropdown">
      {{ selectedLabel }}
      <span class="arrow" :class="{ 'arrow-up': isDropdownOpen }"></span>
    </div>
    <div class="select-container" v-show="isDropdownOpen">
      <div class="dropdown">
        <div
          v-for="item in options"
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
    value: String,
    options: {
      type: Array,
      default: () => [],
    },
  },
  data() {
    return {
      selectedOptions: this.value ? [this.value] : [],
      isDropdownOpen: false,
    };
  },
  computed: {
    selectedLabel() {
      return this.selectedOptions.length > 0
        ? this.selectedOptions[0]
        : "Выберите...";
    },
  },
  watch: {
    value(newVal) {
      this.selectedOptions = newVal ? [newVal] : [];
    },
    selectedOptions() {
      this.$emit("input", this.selectedOptions[0] || "");
    },
  },
  methods: {
    toggleOption(item) {
      this.selectedOptions = this.selectedOptions.includes(item) ? [] : [item];
      this.isDropdownOpen = false;
    },
    toggleDropdown() {
      this.isDropdownOpen = !this.isDropdownOpen;
    },
  },
};
</script>

<style scoped lang="scss">
.custom-select {
  width: 100%;
  position: relative;

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

    .arrow {
      margin-left: 5px;
      border: solid #000; /* Черная граница */
      border-width: 0 2px 2px 0;
      display: inline-block;
      padding: 3px;
      transform: rotate(45deg);
      transition: transform 0.3s ease;
    }

    .arrow-up {
      transform: rotate(-135deg);
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
        background-color: #4caf50; /* Зеленый цвет */
        color: #fff; /* Белый цвет текста */
      }
    }
  }

  .isDropdownOpen .arrow {
    transform: rotate(-135deg);
  }
}
</style>
