<template>
  
  <form @submit.prevent="submit">
    
    <!-- Subject -->
    <TextInput
      id="subject"
      label="Subject :"
      placeholder="Email Subject"
      v-model:value="subject"
      :required="true"
      :error="subjectError"
    />

    <!-- Description -->
    <TextAreaInput
      id="description"
      label="Description :"
      placeholder="Email Description"
      v-model:value="description"
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
  import TextAreaInput from './TextAreaInput.vue'

  const subject = ref('');
  const subjectError = ref('');

  const description = ref('');
  const descriptionError = ref('');

  // Validates the subject field.
  const validateSubject = () => {
    if (!subject.value.trim() )  {
      subjectError.value = 'Subject is required'; 
      return false;
    }

    subjectError.value = '';
    return true;
  };

  // Validates the subject field.
  const validateDescription = () => {
    if (!description.value.trim() )  {
      descriptionError.value = 'Description is required'; 
      return false;
    }

    if (description.value.trim().length < 10) {
      descriptionError.value = 'Description must be 10 characters'
    }

    descriptionError.value = '';
    return true;
  };

  // Submits the form
  const submit = () => {
    
    if (validateSubject() && validateDescription()) {
      const formData = {
        subject: subject.value,
        description: description.value
      };
      console.log('Form Submitted !')
    } 
    else {
      console.log('Validation failed !')
    }
};


</script>