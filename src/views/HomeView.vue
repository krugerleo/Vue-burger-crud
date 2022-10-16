<template>
  <Banner/>
  <div class="main-container">
    <h1>Monte o seu burger:</h1>
    <BurgerForm/>
  </div>
</template>

<script>
// @ is an alias to /src
import Banner from '@/components/Banner.vue'
import BurgerForm from '@/components/BurgerForm.vue'

export default {
  name: 'HomeView',
  components: {
    Banner,
    BurgerForm
  }
}
</script>
