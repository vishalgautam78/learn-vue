
<script setup>
import { ref, reactive } from 'vue'

const count = ref(10)

const user = reactive({
  name: 'Vishal',
  age: 40
})

const increment = () => {
  count.value++
}

const updateName = () => {
  user.name = 'Developer'
}
</script>

<template>
  <div>
    <h2>Count: {{ count }}</h2>

    <button @click="increment">
      Increment
    </button>

    <hr />

    <h2>Name: {{ user.name }}</h2>
    <h2>Age: {{ user.age }}</h2>

    <button @click="updateName">
      Update Name
    </button>
  </div>
</template>



