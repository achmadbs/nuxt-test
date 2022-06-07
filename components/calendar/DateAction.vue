<template>
  <div class="calendar__header">
    <div class="header__action">
      <p @click="selectPrevious">prev</p>
      <div>{{ selectedMonth }}</div>
      <p @click="selectNext">next</p>
    </div>
    <div>
      <button class="styled__button" @click="$emit('showModal', true)">
        <img src="~/assets/images/Union.png" alt="asd" />
        Add Event
      </button>
    </div>
  </div>
</template>

<script>
import dayjs from "dayjs";

export default {
  props: {
    currentMonth: {
      type: Object,
      required: true,
    },
  },
  computed: {
    selectedMonth() {
      return this.currentMonth.format("MMMM YYYY");
    },
  },
  methods: {
    selectPrevious() {
      let newMonth = dayjs(this.currentMonth).subtract(1, "month");
      this.$emit("selectedMonth", newMonth);
    },
    selectNext() {
      let newMonth = dayjs(this.currentMonth).add(1, "month");
      this.$emit("selectedMonth", newMonth);
    },
  },
};
</script>

<style scoped>
.calendar__header {
  font-size: 1.5rem;
  font-weight: bold;
  display: flex;
  align-items: center;
  margin-bottom: 1rem;
}
.header__action {
  display: flex;
  flex: 1;
  justify-content: center;
  gap: 1rem;
}
.header__action > p {
  cursor: pointer;
}
.styled__button {
  background-color: var(--main-color);
  border: none;
  padding: 8px;
  border-radius: 5px;
  color: #ffff;
  display: flex;
  align-items: center;
  gap: 0.5rem;
}
</style>
