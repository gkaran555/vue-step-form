<template>
  <div id="app" class="container mx-auto py-6">
    <header>
      <div>
        <span >ADD USER</span>
      </div>
    </header>

    <div class="content">
      <FormWizard/>
    </div>
  </div>
</template>

<script>
import FormWizard from './components/FormWizard.vue'

export default {
  name: 'app',
  components: {
    FormWizard
  }
}
</script>