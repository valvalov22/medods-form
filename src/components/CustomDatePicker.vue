<template>
  <div class="custom-date-picker" ref="datePicker">
    <div class="date-input-container">
      <input
        :value="selectedDate ? getFormattedDate(selectedDate) : ''"
        type="text"
        @focus="showDatePicker"
        @blur="onBlur"
        :placeholder="placeholder"
        readonly
      />
      <div class="date-picker" v-show="isDatePickerOpen" @click.stop>
        <div class="header">
          <button class="arrow" @click="changeYear(-1)">&lt;&lt;</button>
          <button class="arrow" @click="changeMonth(-1)">&lt;</button>
          <span class="month-year" :style="{ width: monthYearWidth }">
            {{ currentMonth }}
          </span>
          <button class="arrow" @click="changeMonth(1)">&gt;</button>
          <button class="arrow" @click="changeYear(1)">&gt;&gt;</button>
        </div>
        <div class="calendar">
          <div class="days-header">
            <div v-for="day in daysOfWeek" :key="day" class="day">
              {{ day }}
            </div>
          </div>
          <div
            v-for="week in calendar"
            :key="week[0].toISOString()"
            class="week"
          >
            <div
              v-for="day in week"
              :key="day.toISOString()"
              @click="selectDate(day)"
              :class="{
                selected: isDateSelected(day),
                today: isToday(day),
                'other-month': isOtherMonth(day),
              }"
              class="day"
            >
              {{ getFormattedDay(day) }}
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      selectedDate: null,
      isDatePickerOpen: false,
      currentMonth: "",
      calendar: [],
      selectedYear: new Date().getFullYear(),
      monthYearWidth: "120px", // Фиксированная ширина
      placeholder: "Выберите дату",
    };
  },
  computed: {
    daysOfWeek() {
      return ["Вс", "Пн", "Вт", "Ср", "Чт", "Пт", "Сб"];
    },
  },
  watch: {
    selectedDate() {
      this.updateCalendar();
    },
    selectedYear() {
      this.updateCalendar();
    },
  },
  methods: {
    showDatePicker() {
      this.isDatePickerOpen = true;
      this.updateCalendar();
      document.addEventListener("click", this.handleClickOutside);
    },
    onBlur() {
      if (!this.isDatePickerOpen) {
        document.removeEventListener("click", this.handleClickOutside);
      }
    },
    handleClickOutside(event) {
      if (
        this.$refs.datePicker &&
        !this.$refs.datePicker.contains(event.target)
      ) {
        this.isDatePickerOpen = false;
        document.removeEventListener("click", this.handleClickOutside);
      }
    },
    updateCalendar() {
      const date = this.selectedDate ? new Date(this.selectedDate) : new Date();
      date.setFullYear(this.selectedYear);
      this.currentMonth = this.getMonthYearString(date);
      this.calendar = this.generateCalendar(date);
    },
    generateCalendar(date) {
      const startOfMonth = new Date(date.getFullYear(), date.getMonth(), 1);
      const endOfMonth = new Date(date.getFullYear(), date.getMonth() + 1, 0);
      const startDate = new Date(startOfMonth);
      startDate.setDate(startDate.getDate() - startDate.getDay());
      const endDate = new Date(endOfMonth);
      endDate.setDate(endDate.getDate() + (6 - endDate.getDay()));
      const calendar = [];
      while (startDate <= endDate) {
        const week = [];
        for (let i = 0; i < 7; i++) {
          week.push(new Date(startDate));
          startDate.setDate(startDate.getDate() + 1);
        }
        calendar.push(week);
      }
      return calendar;
    },
    isDateSelected(day) {
      return this.selectedDate && this.isSameDay(day, this.selectedDate);
    },
    isToday(day) {
      const today = new Date();
      return this.isSameDay(day, today);
    },
    isOtherMonth(day) {
      const currentDate = this.selectedDate
        ? new Date(this.selectedDate)
        : new Date();
      return day.getMonth() !== currentDate.getMonth();
    },
    selectDate(day) {
      this.selectedDate = new Date(day);
      this.$nextTick(() => {
        this.$emit("input", this.selectedDate);
      });
      this.isDatePickerOpen = false;
      document.removeEventListener("click", this.handleClickOutside);
    },
    changeMonth(offset) {
      const currentDate = this.selectedDate
        ? new Date(this.selectedDate)
        : new Date();
      const newDate = new Date(
        currentDate.getFullYear(),
        currentDate.getMonth() + offset,
        currentDate.getDate()
      );
      this.selectedDate = newDate;
      this.$emit("prevent");
    },
    changeYear(offset) {
      this.selectedYear += offset;
      this.$emit("prevent");
    },
    getMonthYearString(date) {
      const monthNames = [
        "Январь",
        "Февраль",
        "Март",
        "Апрель",
        "Май",
        "Июнь",
        "Июль",
        "Август",
        "Сентябрь",
        "Октябрь",
        "Ноябрь",
        "Декабрь",
      ];
      return `${monthNames[date.getMonth()]} ${date.getFullYear()}`;
    },
    getFormattedDate(date) {
      const day = date.getDate();
      const month = date.getMonth() + 1;
      const year = date.getFullYear();
      return `${day < 10 ? "0" : ""}${day}.${
        month < 10 ? "0" : ""
      }${month}.${year}`;
    },
    getFormattedDay(date) {
      return date.getDate();
    },
    isSameDay(date1, date2) {
      return (
        date1.getDate() === date2.getDate() &&
        date1.getMonth() === date2.getMonth() &&
        date1.getFullYear() === date2.getFullYear()
      );
    },
  },
};
</script>

<style scoped lang="scss">
.custom-date-picker {
  display: flex;
  flex-direction: column;
  width: 100%;

  .date-input-container {
    position: relative;
    width: 100%;

    input {
      width: 100%;
      padding: 8px;
      box-sizing: border-box;
      margin-top: 5px;
      margin-bottom: 10px;
      border: 1px solid #ccc;
      border-radius: 4px;
      cursor: pointer;
    }

    .date-picker {
      z-index: 1;
      position: absolute;
      top: 100%;
      left: 0;
      width: 100%;
      background-color: #fff;
      border: 1px solid #3498db;
      border-top: none;
      border-radius: 0 0 4px 4px;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
      display: flex;
      flex-direction: column;

      .header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        background-color: #3498db;
        color: #fff;
        padding: 8px;
        font-weight: bold;

        .arrow {
          background: none;
          border: none;
          color: #fff;
          cursor: pointer;
          font-size: 1em;
          outline: none;
        }
      }

      .days-header {
        display: grid;
        grid-template-columns: repeat(7, 1fr);
        background-color: #f4f4f4;
        padding: 8px;
        font-weight: bold;

        .day {
          text-align: center;
        }
      }

      .calendar {
        flex: 1;
        display: grid;
        grid-template-columns: repeat(1, 1fr);
        padding: 8px;

        .week {
          display: grid;
          grid-template-columns: repeat(7, 1fr);

          .day {
            text-align: center;
            padding: 8px;
            cursor: pointer;
            transition: background-color 0.3s;

            &:hover {
              background-color: #e0e0e0;
            }

            &.selected {
              background-color: #3498db;
              color: #fff;
              border-radius: 50%;
            }

            &.today {
              font-weight: bold;
            }

            &.other-month {
              color: #ccc;
            }
          }
        }
      }
    }
  }
}
</style>
