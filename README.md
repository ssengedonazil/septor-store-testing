# Vue 3 + TypeScript + Vite

# septor-store

Description (Full)
septor-store is a structured and scalable state management solution built on top of Pinia for Vue 3. It embraces the Plain Old Module (POM) pattern to simplify how developers—junior or senior—write, organize, and scale their application state. Whether you're just starting out or architecting large Vue applications, this tool helps you keep your stores clean, predictable, and easy to maintain.

### Features

### Add Quick Summary Table

| Feature                                   | Description                                  |
|-------------------------------------------|----------------------------------------------|
| API Deduplication                         | Prevents duplicate in-flight API calls       |
| State Sync                                | Dynamically generates Pinia state per API    |
| Caching                                   | SessionStorage support                       |
| Pagination Helpers                        | Stubbed pagination extractors                |
| Post + Submit Support                     | Easily manage POST data and track responses  |
| Flexible Callbacks                        | Use custom callbacks on state update         |
| Loading & Error States                    | Built-in support for loading/errors          |

- Automatic API Call Queue Management Prevents multiple simultaneous calls to the same endpoint by tracking ongoing requests.
- Session Storage Caching Support Optionally caches API responses in sessionStorage for faster retrieval and offline resilience.
- Dynamic State Generation Creates and manages state slices dynamically based on API response keys, supporting scalable state organization.
- Paginated Data Handling Includes helper methods to extract page numbers from URLs and manage paginated API responses (stubbed but     extensible).
- State Reload and Refresh Controls            Supports forced reload of API data with configurable delay timers to prevent rapid repeat requests.
- Flexible Callback Integration Allows injection of custom callback functions to extend or modify store behavior after state updates.
- Error Handling and Logging Centralizes error capturing and logs API call issues for easier debugging and maintenance.
- Loading State Management Automatically tracks and exposes loading status for UI feedback during asynchronous operations.
- Generic Data Validation Includes utility methods to validate object/array length and type, improving robustness.
- Built-in Sleep/Delay Utility Supports asynchronous delay for throttling or debouncing API requests.
- Fast, cache-friendly state access
- Lightweight reactivity
- Data-ready views
- Seamless session recovery

---

### Features (comming Soon)

**Here are some possible future features to consider adding and highlighting:**

- Optimistic UI Updates to immediately reflect user changes before API confirmation.
- Automatic Retry Mechanism for failed API calls with configurable backoff.
- Global State Reset and Clear Functions for user logout or data purge scenarios.
- Integration with Vue Devtools for enhanced debugging.
- TypeScript Support fully typed throughout for safer development.
- Support for Different Storage Backends like localStorage, IndexedDB, or custom persistence.
- Reduces server load and ensures consistent state by ignoring duplicate in-flight requests.

---

### Note

- This package is designed with both junior and senior developers in mind, making state management and API handling effortless and reliable. By intelligently preventing unnecessary and duplicate API calls, it optimizes your app’s performance and reduces network overhead — a huge win for user experience and resource management.
It empowers your application with dynamic, flexible state generation and smart caching, so your data is always fresh yet efficiently retrieved. The built-in loading states and error handling simplify complex async workflows, letting you focus more on building features rather than wrestling with data consistency.
Whether you're building a small project or scaling a large application, septor-store offers a clean, extensible, and maintainable approach to state management that saves you time, prevents bugs, and keeps your codebase organized.

---

### can be ignored

```bash
setBearerToken({token:hellothere}); set Bearer token object 
getBearerToken(); get Bearer token object
*❌ These will now throw errors: *
setBearerToken(null);
setBearerToken("string");
setBearerToken(123);
setBearerToken(["array"]);

*✅ These will now :*
setBearerToken({ token: "abc123" });
// Or with additional props
setBearerToken({ token: "abc123", expiresIn: 3600 });
```

### Installation

```bash
npm install septor-store

# septor-store
```

### Usage **EXAMPLE 1**

.env file

variable

 VITE_BACKEND_URL=<http://127.0.0.1:8000/api>
always  api will come like this

## Step 1

<http://127.0.0.1:8000/api/and-your-endpoint>

```bash
// main.ts
import { createApp } from 'vue';
import { createPinia } from 'pinia';
import App from './App.vue';
const app = createApp(App);
const pinia = createPinia();
app.use(pinia);
```

## Step 2

```bash
// functions.ts
import { pomPinia,setBearerToken,getBearerToken } from "septor-store"

const usePomStore = pomPinia('myCustomStoreId')|| pomPinia();
    const pomStore = usePomStore();
    // Example: Call API and store data
    const fetchData = async () => {
      const data = await pomStore.stateGenaratorApi({
        reload: true,// if false ,Once the data is colleted  never call again  And if true   call the data in all stuation
        StateStore: 'users',
        reqs: { url: '/api/users', method: 'get',data:{} }, //data:{sex:male} is not required
        time: 5,// if the state not empty  time taken to recall again the api
      });
      console.log('Fetched data:', data);
    };
    return {fetchData};
  }
```

## Step 3

```bash
componets.vue
<script>
import {onMounted} from 'vue'
import { pomPinia,setBearerToken,getBearerToken } from "septor-store"

const usePomStore = pomPinia('myCustomStoreId')|| pomPinia();
import fetchData from './functions.ts'
onMounted(()=>{
    fetchData()
})
</script>
  <template>
  <div>
    <!-- all the users  result  bellow -->
    <h1> {{ fetchData.users }}</h1>  
  </div>
</template>
```

### Usage **EXAMPLE 2**

```bash
# mUse avoid using this  while posting data 
<script setup>
import { onMounted, ref } from 'vue'
import { pomPinia,setBearerToken,getBearerToken } from "septor-store"

const pomStore = pomPinia('user-post-demo')
// Create your POM store instance 😇 

// Reactive reference for user list
const users = ref([])

onMounted(async () => {
  await pomStore.stateGenaratorApi({
    StateStore: 'users',
    reqs: { url: '/api/users', method: 'get', },
    # mStore: { mUse: true },
  })

  // Fetch the result from the state
  users.value = pomStore.users?.payload || []
})
</script>

<template>
  <div>
    <h2 class="text-xl font-bold mb-4">👥 User List</h2>
    <div v-if="pomStore.Loading" class="text-blue-500">Loading users...</div>
    <ul v-else>
      <li v-for="user in users" :key="user.id" class="mb-2">
        {{ user.name }} - {{ user.email }}
      </li>
    </ul>
  </div>
</template>

<style scoped>
ul {
  list-style-type: none;
  padding: 0;
}
</style>


~~~
### Usage **EXAMPLE 3**  data quick access 😃

```bash
store.stateGenaratorApi(
  {
    StateStore: 'users',
    reqs: { url: '/api/users', method: 'get' },
    // mStore: { mUse: true }, //  mStore: { mUse: true } is not required
  },
  (existingData) => {
    console.log('💡 Instant access to cached/previous data:', existingData)
    
    if (existingData?.length) {
      renderUsers(existingData)
    }
  }
)

```

### Usage **EXAMPLE 3**  Posting data 🤭

``` bash
<template>
  <div>
    <h2>Add User</h2>
    <form @submit.prevent="handleSubmit">
      <input v-model="user.name" placeholder="Name" required />
      <input v-model="user.email" placeholder="Email" required />
      <button :disabled="store.Loading">Submit</button>
    </form>

    <div v-if="store.Loading">Loading...</div>
    <div v-if="result">Success: {{ result.message }}</div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { pomPinia,setBearerToken,getBearerToken } from "septor-store"

const store = pomPinia('user-post-demo')
const user = ref({ name: '', email: '' })
const result = ref(null)

const handleSubmit = async () => {
  const config = {
    StateStore: 'createUserResult',
    reqs: {
      url: '/api/users', // your POST endpoint
      method: 'post',
      data: user.value,
    },
    // mStore: { mUse: false },  can be removed 
    reload: true,
  }

  const res = await store.stateGenaratorApi(config, (oldData) => {
    console.log('Previous post result if any:', oldData)
  })


  result.value = res
}
</script> 

```
