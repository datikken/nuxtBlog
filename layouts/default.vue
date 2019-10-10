<template>
  <div>
    <Navbar />
    <main>
      <div class="container">
        <nuxt />
      </div>
    </main>
  </div>
</template>

<script>
import Navbar from '../components/navbar';

export default {
  components: {
    Navbar
  }
}
</script>
<style>

</style>
