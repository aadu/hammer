<template>
  <div class="home p-d-flex">
    <QuestionTable  />
  </div>
</template>

<script>
// @ is an alias to /src
import QuestionTable from '@/components/QuestionTable.vue'

export default {
  name: 'Home',
  components: {
    QuestionTable
  }
}
</script>
