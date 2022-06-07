<template>
  <div>
    <RenderHeader
      :currentMonth="selectedMonth"
      @selectedMonth="handleChangeMonth"
      @showModal="handleToggleModal"
    />
    <RenderDays />
    <div class="days__grid">
      <RenderDaysOfMonth v-for="day in days" :key="day.date" :day="day" />
    </div>
    <RenderModal
      :isModalShow="isModalShow"
      @closeModal="handleToggleModal"
      :eventDate.sync="eventDate"
      :eventName.sync="eventName"
      :handleSubmitEvent="handleSubmitEvent"
    />
  </div>
</template>

<script>
import dayjs from "dayjs";
import weekday from "dayjs/plugin/weekday";
import weekOfYear from "dayjs/plugin/weekOfYear";
import RenderDaysOfMonth from "./DaysOfMonth";
import RenderHeader from "./DateAction";
import RenderDays from "./Days";
import RenderModal from "../Modal.vue";

dayjs.extend(weekday);
dayjs.extend(weekOfYear);

export default {
  name: "CalendarMonth",

  components: {
    RenderDaysOfMonth,
    RenderHeader,
    RenderDays,
    RenderModal,
  },

  data() {
    return {
      selectedMonth: dayjs(),
      isModalShow: false,
      listEvents: [],
      eventName: "",
      eventDate: "",
    };
  },

  computed: {
    days() {
      return [
        ...this.previousMonthDays,
        ...this.currentMonthDays,
        ...this.nextMonthDays,
      ];
    },
    month() {
      return Number(this.selectedMonth.format("M"));
    },
    year() {
      return Number(this.selectedMonth.format("YYYY"));
    },
    numberOfDaysInMonth() {
      return dayjs(this.selectedMonth).daysInMonth();
    },
    currentMonthDays() {
      return [...Array(this.numberOfDaysInMonth)].map((_, index) => {
        return {
          date: dayjs(`${this.year}-${this.month}-${index + 1}`).format(
            "YYYY-MM-DD"
          ),
          isCurrentMonth: true,
          events: this.listEvents.reduce((acc, curr) => {
            const currentFullDate = dayjs(
              `${this.year}-${this.month}-${index + 1}`
            ).format("YYYY-MM-DD");
            if (
              dayjs(curr.eventDate).format("YYYY-MM-DD") === currentFullDate
            ) {
              acc["eventName"] = curr.eventName;
              acc["eventDate"] = curr.eventDate;
            }
            return acc;
          }, {}),
        };
      });
    },
    previousMonthDays() {
      const firstDayOfTheMonthWeekday = this.getWeekday(
        this.currentMonthDays[0].date
      );
      const previousMonth = dayjs(`${this.year}-${this.month}-01`).subtract(
        1,
        "month"
      );

      const visibleNumberOfDaysFromPreviousMonth = firstDayOfTheMonthWeekday
        ? firstDayOfTheMonthWeekday - 1
        : 6;

      const previousMonthLastMondayDayOfMonth = dayjs(
        this.currentMonthDays[0].date
      )
        .subtract(visibleNumberOfDaysFromPreviousMonth, "day")
        .date();

      return [...Array(visibleNumberOfDaysFromPreviousMonth)].map(
        (_, index) => {
          return {
            date: dayjs(
              `${previousMonth.year()}-${previousMonth.month() + 1}-${
                previousMonthLastMondayDayOfMonth + index
              }`
            ).format("YYYY-MM-DD"),
            events: {},
          };
        }
      );
    },

    nextMonthDays() {
      const lastDayOfTheMonthWeekday = this.getWeekday(
        `${this.year}-${this.month}-${this.currentMonthDays.length}`
      );

      const nextMonth = dayjs(`${this.year}-${this.month}-01`).add(1, "month");

      const visibleNumberOfDaysFromNextMonth = lastDayOfTheMonthWeekday
        ? 7 - lastDayOfTheMonthWeekday
        : lastDayOfTheMonthWeekday;

      return [...Array(visibleNumberOfDaysFromNextMonth)].map((_, index) => {
        return {
          date: dayjs(
            `${nextMonth.year()}-${nextMonth.month() + 1}-${index + 1}`
          ).format("YYYY-MM-DD"),
          events: {},
        };
      });
    },
  },

  methods: {
    getWeekday(date) {
      return dayjs(date).weekday();
    },
    handleChangeMonth(newSelectedMonth) {
      this.selectedMonth = newSelectedMonth;
    },
    handleToggleModal(newModalState) {
      this.isModalShow = newModalState;
    },
    handleSubmitEvent() {
      if (!this.eventName) {
        return alert("name cannot be blank");
      }
      this.listEvents.push({
        eventName: this.eventName,
        eventDate: this.eventDate,
      });
      this.eventName = "";
      this.eventDate = "";
      this.isModalShow = false;
    },
  },
};
</script>

<style scoped>
.days__grid {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  height: 100%;
}
</style>
