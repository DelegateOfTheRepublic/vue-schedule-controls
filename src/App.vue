<template>
  <div class="view">
    <p>
      Selected option:
      {{ selectedOption.length ? selectedOption : "No selected" }}
    </p>
    <v-select
      :title="'Студенты'"
      :options="options"
      v-model:value="selectedOption"
      @input="(values) => (selectedOption = values)"
    />
    <date-range-picker @change="(value) => (dateRange = value)" />
    {{
      isEmptyDateRange
        ? "Nothing"
        : `${dateRange.startDate?.week}~${dateRange.endDate?.week}`
    }}
  </div>
</template>

<script>
import VSelect from "./components/VSelect.vue";
import DateRangePicker from "./components/DateRangePicker.vue";
import _ from "lodash";

export default {
  name: "App",
  components: {
    VSelect,
    DateRangePicker,
  },
  data() {
    return {
      options: [
        { id: 0, name: "Student A B 123" },
        { id: 1, name: "Student C D" },
        { id: 2, name: "Student E F" },
        { id: 3, name: "Student G H" },
        { id: 4, name: "Student I J" },
      ],
      selectedOption: [
        { id: 0, name: "Student A B 123" },
        { id: 3, name: "Student G H" },
      ],
      dateRange: {},
    };
  },
  computed: {
    isEmptyDateRange() {
      return _.isEmpty(this.dateRange);
    },
  },
};
</script>

<style>
* {
  box-sizing: border-box;
}

.view {
  display: flex;
  flex-direction: column;
  align-items: center;
}

body {
  margin: 0;
  width: 100%;
  min-height: 100vh;
  overflow: auto;
}

ul {
  margin: 0;
  padding: 0;
}

li {
  list-style-type: none;
}

p {
  margin: 0;
}

#app {
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
