<template>
  <div class="date-wrapper">
    <div
      class="date-range-picker__label"
      @click="showDatePicker = !showDatePicker"
    >
      <span>{{ formattedDateRange }}</span>
    </div>
    <div class="date-range-picker" v-if="showDatePicker">
      <div class="date-picker__col">
        <p>Верхние недели</p>
        <div
          class="date-picker__date"
          v-for="(week, i) in dates.topWeeks"
          :key="i"
          @click="setDate('startDate', week)"
          :data-selected="isSelectedDate('startDate', week)"
        >
          <p>{{ week.week }}</p>
          <p>{{ week.weekNum }}-я неделя</p>
        </div>
      </div>
      <div class="date-picker__col">
        <p>Нижние недели</p>
        <div
          class="date-picker__date"
          v-for="(week, i) in dates.bottomWeeks"
          :key="i"
          @click="setDate('endDate', week)"
          :data-selected="isSelectedDate('endDate', week)"
        >
          <p>{{ week.week }}</p>
          <p>{{ week.weekNum }}-я неделя</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import _ from "lodash";

export default {
  data() {
    return {
      dates: {
        bottomWeeks: [
          { weekNum: 21, week: "13.02 - 18.02" },
          { weekNum: 23, week: "27.02 - 04.03" },
          { weekNum: 25, week: "13.03 - 18.03" },
          { weekNum: 27, week: "27.03 - 01.04" },
          { weekNum: 29, week: "10.04 - 15.04" },
        ],
        topWeeks: [
          { weekNum: 20, week: "06.02 - 11.02" },
          { weekNum: 22, week: "20.02 - 25.02" },
          { weekNum: 24, week: "06.03 - 11.03" },
          { weekNum: 26, week: "20.03 - 25.03" },
          { weekNum: 28, week: "03.04 - 08.04" },
        ],
      },
      placeholder: "Click to pick date range...",
      dateRange: {
        startDate: {},
        endDate: {},
      },
      showDatePicker: false,
    };
  },
  methods: {
    checkDateRange() {
      const { startDate, endDate } = this.dateRange;

      if (startDate?.weekNum > endDate?.weekNum) {
        return { startDate: endDate, endDate: startDate };
      }

      return this.dateRange;
    },
    setDate(dateRangeKey, newDate) {
      if (
        _.isEmpty(this.dateRange[dateRangeKey]) ||
        this.dateRange[dateRangeKey] !== newDate
      ) {
        this.dateRange[dateRangeKey] = newDate;
      } else {
        delete this.dateRange[dateRangeKey];
      }

      this.$emit("change", this.checkDateRange());
    },
    isSelectedDate(dateRangeKey, date) {
      return this.dateRange[dateRangeKey] === date;
    },
  },
  computed: {
    formattedDateRange() {
      let { startDate, endDate } = this.dateRange;

      if (_.isEmpty(startDate) && _.isEmpty(endDate)) {
        return !this.showDatePicker ? this.placeholder : "Please pick a dates";
      }

      if (startDate?.weekNum > endDate?.weekNum) {
        return `${endDate.week} ~ ${startDate.week}`;
      }

      return `${startDate?.week || "Начальная дата"} ~ ${
        endDate?.week || "Конечная дата"
      }`;
    },
  },
};
</script>

<style scoped>
.date-wrapper {
}

.date-range-picker__label {
  position: relative;
  border-radius: 6px;
  padding: 2px 6px;
  cursor: pointer;

  background: cornflowerblue;
}

.date-range-picker {
  position: absolute;
  right: 0;
  left: 0;
  margin: auto;
  margin-top: 30px;
  width: fit-content;
  height: fit-content;
  display: flex;
  gap: 25px;
  border-radius: 6px;
  align-items: center;

  background: cornflowerblue;
}

.date-picker__col {
  padding: 4px 8px;
}

.date-picker__date {
  padding: 2px 4px;
  width: fit-content;
  margin: auto;
}

.date-picker__date[data-selected="true"] {
  background: lightslategray;
}

.date-picker__date:hover {
  border-radius: 6px;
  background: dodgerblue;
  cursor: pointer;
}
</style>