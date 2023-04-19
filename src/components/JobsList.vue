<template>
  <div class="job-list">
    <p>Ordered by {{ order }}</p>
    <transition-group name="list" tag="ul">
      <li v-for="job in orderedJobs" :key="job.id">
        <h2>{{ job.title }} in {{ job.location }}</h2>
        <div class="salary">
          <img width="75" src="src/assets/rupee.svg" alt="rupia-icon">
          <p>{{ job.salary }} rupias</p>
        </div>
        <div class="description">
          <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum</p>
        </div>
      </li>
    </transition-group>
  </div>
</template>

<script lang="ts">
import {computed, defineComponent, PropType} from "vue";
import Job from "@/types/job";
import OrderTerm from "@/types/OrderTerm";

export default defineComponent({
  props: {
    jobs: {
      required: true,
      type: Array as PropType<Job[]>,
    },
    order: {
      required: true,
      type: String as PropType<OrderTerm>
    }
  },
  setup(props) {
    const orderedJobs = computed(() => {
      // with the [...array] we have a "temporary array", and not directly editing the array from prop
      return [...props.jobs].sort((a: Job, b: Job) => {
        return props.order === "salary"
          ? a[props.order] < b[props.order] ? 1 : -1  // a[attr] is equal to a.attr, but implements dynamic attributes
          : a[props.order] > b[props.order] ? 1 : -1
      })
    })

    return {orderedJobs}
  }
})

</script>

<style scoped>
.job-list {
  max-width: 960px;
  margin: 40px auto;
}
ul {
  padding: 0;
}
li {
  list-style-type: none;
  background: white;
  padding: 16px;
  margin: 16px 0;
  border-radius: 10px;
}
.job-list h2 {
  margin: 10px 100px 10px 20px;
  text-transform: capitalize;
}
.salary {
  display: flex;
}
.salary p {
  color: #17bf66;
  font-weight: bold;
  margin: 10px 4px;
}
.list-move {
  transition: all 0.75s;
}

</style>