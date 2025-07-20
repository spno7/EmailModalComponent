<template>
  
  <form @submit.prevent=" " >
    
    <TextInput
      id="subject"
      label="Subject :"
      placeholder="Email Subject"
      v-model:value="subject"
      :required="true"
      :error="subjectError"
    />

    <button type="submit" class="btn btn-primary">
      SUBMIT
    </button>

  </form>
</template>

<script setup>

  import { ref } from 'vue';
  import TextInput from './TextInput.vue'

  const subject = ref('');
  const subjectError = ref('');

  const description = ref('');
  const descriptionError = ref('');

  // Validates the subject field.
  const validateSubject = () => {
    if (!subject.value.trim() )  {
      subjectError.value = 'Subject is required.'; 
      return false;
    }

    subjectError.value = '';
    return true;
  };

</script>

<style scoped>

</style>
