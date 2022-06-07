<template>
  <ol class="day-of-week">
    <li v-for="weekday in week" :key="weekday">
      <p>{{ weekday }}</p>
    </li>
  </ol>
</template>

<script>
export default {
  name: "Days",
  data() {
    return {
      week: [
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
        "Sunday",
      ],
    };
  },
};
</script>

<style scoped>
.day-of-week {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
}

.day-of-week > li {
  background-color: #f5f5f5;
  text-align: center;
  border: 1px solid #dddddd;
}

.day-of-week > li > p {
  padding: 8px;
  font-weight: 700;
}
</style>
