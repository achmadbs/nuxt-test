<template>
  <div class="calendar__day">
    <p :class="{ not__current_days: !day.isCurrentMonth }">{{ label }}</p>
    <div
      v-if="day.events !== null && Object.keys(day.events).length !== 0"
      class="event__box"
    >
      <p class="ellipsis__text" @click="clickedEvent">
        {{ day.events.eventName }}
      </p>
    </div>
  </div>
</template>

<script>
import dayjs from "dayjs";

export default {
  name: "CalendarMonthDayItem",
  props: {
    day: {
      type: Object,
      required: true,
    },
    isCurrentMonth: {
      type: Boolean,
      default: false,
    },
  },
  computed: {
    label() {
      return dayjs(this.day.date).format("D");
    },
  },
  mounted() {
    console.log(this.day);
  },
  methods: {
    clickedEvent() {
      alert(`${this.day.events.eventName} \n${this.day.events.eventDate}`);
    },
  },
};
</script>

<style scoped>
.calendar__day {
  height: 97px;
  border-right: 1px solid #dddddd;
  border-bottom: 1px solid #dddddd;
  border-left: 1px solid #dddddd;
  padding: 1rem;
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}
.calendar__day > p {
  text-align: right;
  margin-right: 10px;
  font-weight: 700;
}
.not__current_days {
  color: #dddddd;
}
.event__box {
  background-color: var(--main-color);
  padding: 8px;
  border-radius: 12px;
  color: #fff;
  width: 100px;
  cursor: pointer;
}
.ellipsis__text {
  white-space: nowrap;
  text-overflow: ellipsis;
  overflow: hidden;
}
</style>
