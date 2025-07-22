<template>
  
  <form @submit.prevent="submit">

    <!-- Customer Email -->
    <AutoComplete
      id="customerEmails"
      label="List of recipents :"
      placeholder="Enter Email"
      v-model:selected-emails="customerEmails"
      :error="customerEmailsError"
    />
    
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
      :error="descriptionError"
      :show-char-count="true"
      :max-length="3000"
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
  import AutoComplete from './AutoComplete.vue'

  // Subject 
  const subject = ref('');
  const subjectError = ref('');

  // Description
  const description = ref('');
  const descriptionError = ref('');

  // Emails
  const customerEmails = ref( [] ); 
  const customerEmailsError = ref('');

  // Validates the subject field.
  const validateSubject = () => {
    if (!subject.value.trim() )  {
      subjectError.value = 'Subject is required'; 
      return false;
    }

    subjectError.value = '';
    return true;
  };

  // Validates the description field.
  const validateDescription = () => {
    if (!description.value.trim() )  {
      descriptionError.value = 'Description field is required'; 
      return false;
    }

    if (description.value.trim().length < 10) {
      descriptionError.value = 'Description must be 10 characters';
      return false;
    }

    descriptionError.value = '';
    return true;
  };

  // Validate email
  const validateEmails = () => { 
  
    if (customerEmails.value.length === 0) {
      customerEmailsError.value = 'Enter at least one email'; // More descriptive message
      return false;
    }

    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    
    for (const element of customerEmails.value) { 

      let emailString;

      if (typeof element === "object" && element !== null )
        emailString = element.email
      else 
        emailString = element

      if (!emailRegex.test(emailString)) {
        customerEmailsError.value = `Invalid email format: ${emailString}`;
        return false; 
      }
    }

    customerEmailsError.value = ''; 
    return true;
};


  // Submits the form
  const submit = () => {

    let isValid = true;

    const subjectIsValid = validateSubject();
    const descriptionIsValid = validateDescription();
    const emailIsValid = validateEmails();

    isValid = subjectIsValid && descriptionIsValid && emailIsValid;
    
    if (isValid) {
      const formData = {
        subject: subject.value,
        description: description.value,
        customerEmails: customerEmails.value
      };
      
      console.log('Form Submitted: \n')
      console.log(formData)    
    } 
    else {
      console.log('Validation failed !')
    }
  };

</script>