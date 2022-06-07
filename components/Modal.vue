<template>
  <div v-if="isModalShow" class="modal__container">
    <div class="modal__content">
      <h2>Add Event</h2>
      <div class="form__container">
        <label for="event-name">Event Name</label>
        <input
          type="text"
          id="event-name"
          @change="$emit('update:eventName', $event.target.value)"
        />
      </div>
      <div class="form__container">
        <label for="event-date">Event Date</label>
        <date-picker
          id="event-date"
          input-class="date__picker"
          :format="customFormatter"
          v-model="selectedDate"
        ></date-picker>
      </div>
      <button @click="handleSubmitEvent">Submit</button>
    </div>
  </div>
</template>

<script>
import dayjs from "dayjs";

export default {
  name: "Modal",
  props: {
    isModalShow: {
      type: Boolean,
      default: false,
    },
    handleSubmitEvent: {
      type: Function,
      required: true,
    },
  },
  data() {
    return {
      testDate: "",
    };
  },
  computed: {
    selectedDate: {
      get: function () {
        return this.eventDate;
      },
      set: function (newDate) {
        this.$emit("update:eventDate", dayjs(newDate).format("YYYY-MM-DD"));
      },
    },
  },
  methods: {
    customFormatter(date) {
      return dayjs(date).format("YYYY-MM-DD");
    },
  },
};
</script>

<style>
h2 {
  text-align: center;
  color: var(--main-color);
}
.modal__container {
  position: absolute;
  margin: auto;
  background-color: rgba(0, 0, 0, 0.4);
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  display: flex;
  align-items: center;
  justify-content: center;
}
.modal__content {
  width: 37.5rem;
  background: #ffffff;
  border-radius: 20px;
  padding: 2rem 3.75rem;
  box-sizing: border-box;
  z-index: 5;
}
.form__container {
  display: flex;
  flex-direction: column;
  gap: 6px;
  margin-bottom: 1rem;
}
.form__container > label {
  color: #aaaaaa;
  font-size: 14px;
  line-height: 18px;
}
.form__container > input {
  height: 2.5rem;
  border-radius: 10px;
  border: 1px solid #dddddd;
}
.date__picker {
  height: 2.5rem;
  width: 100%;
  border-radius: 10px;
  border: 1px solid #dddddd;
}
.modal__content > button {
  width: 100%;
  padding: 0.75rem;
  background: var(--main-color);
  border: 1px solid var(--main-color);
  border-radius: 10px;
  color: #fff;
}
</style>
