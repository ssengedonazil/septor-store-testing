<script setup lang="ts">
import { pomPinia,setBearerToken,getBearerToken } from "septor-store"
import { onMounted } from "vue";

setBearerToken({token:'eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOi8vMTI3LjAuMC4xOjgwMDAvYXBpL3YyL2F1dGgvMmZhIiwiaWF0IjoxNzQ3Mzc5NTc2LCJleHAiOjE3NDc0MjI3NzYsIm5iZiI6MTc0NzM3OTU3NiwianRpIjoiUWxuQjltb0F2anp6cXRxNSIsInN1YiI6IjIyIiwicHJ2IjoiY2I0ZWU5OTdiYjIyNTEyMTg0M2NiMmU1M2I3NGM2M2FkM2RlN2I0YiJ9.l7vaIIWhpEcAAKW4b5MKG2ET8AQnmcAcAw1Wr1fBBQg'})

const state = pomPinia()
const fetchData = async () => {
      const data = await state.stateGenaratorApi({
        reload: true,// if false ,Once the data is colleted  never call again  And if true   call the data in all stuation
        StateStore: 'users11',
        reqs: { url: 'v3/patients?Rows=100&status=1&page=1&both=1&branchId=1', method: 'get',data:{} }, //data:{sex:male} is not required
        time: 5,// if the state not empty  time taken to recall again the api
      });
      console.log('Fetched data:', data);
};





onMounted(() => {
  state.counter = 0
  fetchData()
})
</script>

<template>
  <h1>{{ msg }}</h1>

  <div class="card">
    <button type="button" @click="state.counter++">count is {{ state.counter }}</button>
    <p>
         <ul>
        <li >{{ state.users11 }}</li>
      </ul>
      <ul>
        <li v-for="counts in (state.counter??[])"></li>
      </ul>
      <code>components/HelloWorld.vue</code> to test HMR
    </p>
  </div>
 
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
