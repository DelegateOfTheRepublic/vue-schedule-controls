<template>
  <div class="v-select">
    <div class="v-select__input" @click="showSelect = !showSelect">
      <div class="v-select__input-title">
        <template v-if="selectedOptions.length">
          <span
            class="v-select__input-item"
            v-for="(option, i) in selectedOptions"
            :key="i"
            @click.stop
          >
            <span class="input-item__title">{{ option[displayProperty] }}</span>
            <span
              class="input-item__close"
              @click="deselectOption(option[valueProperty])"
            >
              <svg
                xmlns="http://www.w3.org/2000/svg"
                version="1.1"
                xmlns:xlink="http://www.w3.org/1999/xlink"
                xmlns:svgjs="http://svgjs.com/svgjs"
                width="15"
                height="15"
                x="0"
                y="0"
                viewBox="0 0 24 24"
                style="enable-background: new 0 0 512 512"
                xml:space="preserve"
                class=""
              >
                <g>
                  <path
                    fill="#000000"
                    fill-rule="evenodd"
                    d="M5 17.586a1 1 0 1 0 1.415 1.415L12 13.414 17.586 19A1 1 0 0 0 19 17.586L13.414 12 19 6.414A1 1 0 0 0 17.585 5L12 10.586 6.414 5A1 1 0 0 0 5 6.414L10.586 12z"
                    clip-rule="evenodd"
                    data-original="#000000"
                    class=""
                  ></path>
                </g>
              </svg>
            </span>
          </span>
        </template>
        <span v-else>{{ title }}</span>
      </div>
      <span class="v-select__arrow">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          version="1.1"
          xmlns:xlink="http://www.w3.org/1999/xlink"
          xmlns:svgjs="http://svgjs.com/svgjs"
          width="15"
          height="15"
          x="0"
          y="0"
          viewBox="0 0 512 512"
          style="enable-background: new 0 0 512 512"
          xml:space="preserve"
          class=""
        >
          <g>
            <path
              d="m98 190.06 139.78 163.12a24 24 0 0 0 36.44 0L414 190.06c13.34-15.57 2.28-39.62-18.22-39.62h-279.6c-20.5 0-31.56 24.05-18.18 39.62z"
              fill="#000000"
              data-original="#000000"
              class=""
            ></path>
          </g>
        </svg>
      </span>
    </div>
    <div class="v-select__droplist" v-if="showSelect">
      <div class="v-select__filter">
        <div class="v-select__search">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            version="1.1"
            xmlns:xlink="http://www.w3.org/1999/xlink"
            xmlns:svgjs="http://svgjs.com/svgjs"
            width="15"
            height="15"
            x="0"
            y="0"
            viewBox="0 0 512.005 512.005"
            style="enable-background: new 0 0 512 512"
            xml:space="preserve"
            class=""
          >
            <g>
              <path
                d="m505.749 475.587-145.6-145.6c28.203-34.837 45.184-79.104 45.184-127.317C405.333 90.926 314.41.003 202.666.003S0 90.925 0 202.669s90.923 202.667 202.667 202.667c48.213 0 92.48-16.981 127.317-45.184l145.6 145.6c4.16 4.16 9.621 6.251 15.083 6.251s10.923-2.091 15.083-6.251c8.341-8.341 8.341-21.824-.001-30.165zM202.667 362.669c-88.235 0-160-71.765-160-160s71.765-160 160-160 160 71.765 160 160-71.766 160-160 160z"
                fill="#000000"
                data-original="#000000"
              ></path>
            </g>
          </svg>
          <input type="text" placeholder="Search..." v-model="searchQuery" />
        </div>
      </div>
      <div class="v-select__content">
        <p
          v-for="(option, i) in filteredOptions"
          :key="i"
          @click="addOption(option)"
        >
          {{ option[displayProperty] }}
        </p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    title: {
      type: String,
      default: "Click to select",
    },
    options: {
      type: Array,
      default: () => [],
    },
    displayProperty: {
      type: String,
      default: "name",
    },
    valueProperty: {
      type: String,
      default: "id",
    },
    value: {
      default: () => [],
    },
  },
  data() {
    return {
      searchQuery: "",
      showSelect: false,
      selectedOptions: [],
    };
  },
  created() {
    this.selectedOptions = this.$props.value;
  },
  methods: {
    addOption(option) {
      const existOption = this.selectedOptions.findIndex(
        (sOption) => sOption[this.valueProperty] === option[this.valueProperty]
      );

      if (existOption !== -1) {
        this.deselectOption(option[this.valueProperty]);
      } else {
        this.selectedOptions.push(option);
        this.$emit("input", this.selectedOptions);
      }
    },
    deselectOption(key) {
      const result = this.selectedOptions.filter(
        (option) => option[this.valueProperty] !== key
      );
      this.selectedOptions = result;
      this.$emit("input", this.selectedOptions);
    },
  },
  computed: {
    filteredOptions() {
      const query = this.searchQuery.toLowerCase().split(" ");
      if (this.searchQuery === "") {
        return this.options;
      }
      return this.options.filter((option) =>
        option[this.displayProperty]
          .toLowerCase()
          .split(" ")
          .map((word) =>
            query.some((sw) => Object.values(sw).every((v) => word.includes(v)))
          )
          .some((val) => val)
      );
    },
  },
};
</script>

<style scoped>
* {
  box-sizing: border-box;
}

input[type="text"] {
  width: 100%;
  outline: none;
  border: none;
  background: transparent;
}

.v-select {
  margin-top: 40%;
  width: 180px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.v-select__input {
  padding: 4px 8px;
  border: 2px solid blueviolet;
  border-radius: 0.5em;
  position: relative;
  background-color: cornflowerblue;
  display: flex;
  justify-content: space-between;
  cursor: pointer;
}

.v-select__input-item:hover {
  cursor: default;
}

.input-item__title {
  margin-right: 8px;
}

.input-item__close:hover {
  cursor: pointer;
}

.v-select__arrow {
}

.v-select__input-title {
  width: 100%;
  margin-right: 5px;
  display: flex;
  flex-wrap: wrap;
  background-color: darkgrey;
}

.v-select__droplist {
  width: inherit;
  position: absolute;
  transform: translateY(15px);
}

.v-select__filter {
  display: flex;
  flex-direction: column;
  background-color: lightcoral;
  margin-bottom: 8px;
}

.v-select__filter > select {
  width: 100%;
  border: none;
  outline: none;
  background: transparent;
  padding: 4px 8px;
  border: 2px solid blueviolet;
  border-radius: 0.5em;
  margin-bottom: 4px;
}

.v-select__search {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 4px 8px;
  border: 2px solid blueviolet;
  border-radius: 0.5em;
}

.v-select__content {
  background-color: lightsalmon;
}

.v-select__content > p:not(last-of-type) {
  padding: 4px 8px;
  border: 2px solid blueviolet;
  border-radius: 0.5em;
  margin-bottom: 6px;
  cursor: pointer;
}
</style>