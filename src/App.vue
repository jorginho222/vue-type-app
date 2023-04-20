<template>
  <div class="app">
    <header>
      <div class="title">
        <h1>Chirola jobs</h1>
      </div>
      <div class="order">
        <button @click="handleOrder('title')">Order by title</button>
        <button @click="handleOrder('salary')">Order by salary</button>
        <button @click="handleOrder('location')">Order by location</button>
      </div>
      <div v-if="showSalaryInput" class="salary">
        <label>Salario minimo</label>
        <input v-model.number="salary">
        <button @click="filterByMinSalary()">Filtrar</button>
      </div>
    </header>
    <JobsList
      :jobs="jobs" :order="order" :min-salary="minSalary" :show-min-salary="showMinSalary"
      @clean-filter="cleanFilter"
    />
  </div>
</template>

<script lang="ts">
import {defineComponent, ref} from 'vue';
import Job from "@/types/job";
import JobsList from "@/components/JobsList.vue";
import OrderTerm from "@/types/OrderTerm";

export default defineComponent({
  name: 'App',
  components: {
    JobsList
  },
  setup() {
    const jobs = ref<Job[]>([
      { title: 'farm worker', location: 'lon lon ranch', salary: 30000, id: '1' },
      { title: 'quarryman', location: 'death mountain', salary: 40000, id: '2' },
      { title: 'flute player', location: 'the lost woods', salary: 35000, id: '3' },
      { title: 'fisherman', location: 'lake hylia', salary: 21000, id: '4' },
      { title: 'prison guard', location: 'gerudo valley', salary: 32000, id: '5' }
    ])
    const showMinSalary = ref<boolean>(false)
    const showSalaryInput = ref<boolean>(true)
    const order = ref<OrderTerm>('salary')
    const salary = ref<number>(0)
    const minSalary = ref<number>(0)

    const handleOrder = (term: OrderTerm) => {
      order.value = term
    }
    const filterByMinSalary = () => {
      minSalary.value = salary.value
      showSalaryInput.value = false
      showMinSalary.value = true
    }
    const cleanFilter = () => {
      minSalary.value = 0
      salary.value = 0
      showMinSalary.value = false
      showSalaryInput.value = true
    }

    return {jobs,order,salary,minSalary,showMinSalary,showSalaryInput,filterByMinSalary,handleOrder,cleanFilter}
  },

});
</script>

<style>
  header {
    text-align: center;
  }
  header .order {
    margin-top: 20px;
  }
  button {
    margin: 0 10px;
    color: #1195c9;
    border: 3px solid #1195c9;
    background: #d5f0ff;
    padding: 8px 16px;
    border-radius: 4px;
    cursor: pointer;
    font-weight: bold;
  }
  header .title {
    display: flex;
    justify-content: center;
  }
  header h1 {
    font-size: 3em;
  }
  .salary {
    margin-top: 25px;
    display: flex;
    gap: 10px;
    justify-content: center;
    align-items: center;
  }
  .salary input {
    padding: 5px;
    max-width: 100px;
  }

</style>
