<script setup>
defineProps({
  msg: {
    type: String,
    required: true
  }
})
</script>

<template>
  <div>
    <h1>{{ msg }}</h1>
    <h5 style="padding-right:5%">
      You’ve successfully created a project
    </h5>
  </div>
</template>

