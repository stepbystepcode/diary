<template>
  <div class="home">
    <MyDiary />
  </div>
</template>

<script>
// @ is an alias to /src
import MyDiary from '@/components/MyDiary.vue'

export default {
  name: 'HomeView',
  components: {
    MyDiary
  }
}
</script>
