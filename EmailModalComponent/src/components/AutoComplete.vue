<template>
  <div class="mb-5 autocomplete-container position-relative">

    <label v-if="label">
      <b> {{ label }} </b>
    </label>

    <div class="d-flex flex-wrap gap-2 mb-1 p-2 border" 
      v-if="selectedEmails.length > 0"
    >
      <span
        v-for="emailObj in selectedEmails"
        :key="getEmailKey(emailObj)"
        class="email-tag badge bg-primary d-flex align-items-center"
      >

        {{ getEmailText(emailObj) }}
        
        <button
          type="button"
          class="btn-close ms-1"
          aria-label="Remove"
          @click="removeEmail(emailObj)"
        ></button>
      
      </span>
    
    </div>

    <input
      :id="id"
      type="text"
      v-model="searchTerm"
      :placeholder="placeholder"
      @input="handleInput"
      class="form-control rounded-md"
      :class="{ 'is-invalid': error }"
      autocomplete="off"
    />

    <!-- Dropodown Sugesstions -->
    <ul v-if="showSuggestions && filteredSuggestions.length > 0" class="list-group autocomplete-suggestions shadow-sm rounded-md">
      <li
        v-for="(suggestion, index) in filteredSuggestions"
        :key="suggestion.id || suggestion.email"
        class="list-group-item list-group-item-action"
        :class="{ 'active': index === highlightedIndex }"
        @click="selectSuggestion(suggestion)"
      >
        {{ suggestion.name }} ({{ suggestion.email }})
      </li>

    </ul>

    <!-- Display error message -->
    <div v-if="error" class="invalid-feedback d-block"> {{ error }} </div>

  </div>
</template>

<script setup>
  import { ref, computed, onMounted  } from 'vue';

  // Props
  const props = defineProps({
  
    id: {
      type: String, 
      required: true 
    },
    
    label: { 
      type: String, 
      default: '' 
    },
    
    placeholder: { 
      type: String, 
      default: '' 
    },
    
    selectedEmails: { 
      type: Array,
      default: () => []
    },
    
    error: { 
      type: String, 
      default: '' 
    }
  });

  const emit = defineEmits(['update:selectedEmails']);

  const allCustomers = ref([]); 
  const searchTerm = ref('');
  
  const showSuggestions = ref(false);

  const highlightedIndex = ref(-1); 

  const  API_ENDPOINT = 'https://686547495b5d8d0339808f5d.mockapi.io/spitogatos/api/customer-email-lookup';

  let debounceTimeout = null;
  const DEBOUNCE_DELAY = 300; 


  // Get email key
  const getEmailKey = (emailObj) => {
    if (typeof emailObj === 'object' && emailObj !== null) {
      if (emailObj.id) 
        return emailObj.id;
      else
        return emailObj.email;
    }

    return emailObj;
  };

  // Get email text
  const getEmailText = (emailObj) => {
    if (typeof emailObj === 'object' && emailObj !== null) {
      return emailObj.email;
    }

    return emailObj;
  };

  // Fetch all customers from the API.
  const fetchAllCustomers = async () => {
    
    try {
      const response = await fetch(API_ENDPOINT);
      
      if (!response.ok) {
        throw new Error(`Api Error: ${response.status}`);
      }

      allCustomers.value = await response.json();
    }

    catch (err) {
      console.error('Failed to fetch the customers:', err);
    } 
  };

  
  // Returns a Filtered list 
  const filteredSuggestions = computed(() => {
    
    if (!searchTerm.value) 
      return [];

    const search = searchTerm.value.toLowerCase();

    const matches = allCustomers.value.filter(customer => {
      
      const customerEmail = customer.email.toLowerCase();
      const customerName = customer.name.toLowerCase();

      // Check if the customer is already selected
      let alreadySelected = false;
      
      for (const selected of props.selectedEmails) {
        let selectedEmail;
        if (typeof selected === 'object') {
          selectedEmail = selected.email;
        } else {
          selectedEmail = selected;
        
        }
        if (selectedEmail.toLowerCase() === customerEmail) {
          alreadySelected = true;
          break;
        }
      }

      return !alreadySelected && (
        customerName.includes(search) || customerEmail.includes(search)
      );
    });

    return matches.slice(0, 8);
  
  });


  const handleInput = () => {
    clearTimeout(debounceTimeout);
    debounceTimeout = setTimeout(() => {
      showSuggestions.value = true;
      highlightedIndex.value = -1; 
    
    }, DEBOUNCE_DELAY);
  };

  
  // Adds an email to the selected list
  const addEmail = (emailObj) => {
    let emailString;

    if (typeof emailObj === 'object' && emailObj !== null) {
      emailString = emailObj.email;
    } else {
      emailString = emailObj;
    }

   
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    if (!emailRegex.test(emailString)) {
      return;
    }

    const isAlreadySelected = props.selectedEmails.some(selected => {
        let selectedEmail;

        if (typeof selected === 'object' && selected !== null) {
          selectedEmail = selected.email;
        } else {
          selectedEmail = selected;
        }

        return selectedEmail.toLowerCase() === emailString.toLowerCase();
      });
  

      if (!isAlreadySelected) {
        const newSelectedEmails = [...props.selectedEmails, emailObj];
        emit('update:selectedEmails', newSelectedEmails);
      }
    
    showSuggestions.value = false;
    highlightedIndex.value = -1; 
    searchTerm.value = ''; 
  
  };

  // Remove an email
  const removeEmail = (emailObj) => {
    
    let emailString;

    if (typeof emailObj === 'object') {
      emailString = emailObj.email;
    } else {
      emailString = emailObj;
    }

    const newSelectedEmails = props.selectedEmails.filter(selected => {
      let selectedEmail;

      if (typeof selected === 'object') {
        selectedEmail = selected.email;
      } else {
        selectedEmail = selected;
      }

      return selectedEmail.toLowerCase() !== emailString.toLowerCase();
    });

    emit('update:selectedEmails', newSelectedEmails);
  
  };

  // Select a suggestion email 
  const selectSuggestion = (suggestion) => {
    addEmail(suggestion);
  };

 
  const handleClickOutside = (event) => {
    
    if (!event.target.closest('.autocomplete-suggestions') && !event.target.closest('.email-tag')) { 
      
      showSuggestions.value = false;
      highlightedIndex.value = -1;
    }
  };

  // Lifecycle Hook
  onMounted(() => {
    fetchAllCustomers(); 
    
    document.addEventListener('click', handleClickOutside);
  });

</script>

<style scoped>
  
  .autocomplete-container {
    position: relative;
  }

  .btn-close {
    
    background-image: url("data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 16 16' fill='%23fff'%3e%3cpath d='M.293.293a1 1 0 011.414 0L8 6.586 14.293.293a1 1 0 111.414 1.414L9.414 8l6.293 6.293a1 1 0 01-1.414 1.414L8 9.414l-6.293 6.293a1 1 0 01-1.414-1.414L6.586 8 .293 1.707a1 1 0 010-1.414z'/%3e%3c/svg%3e");

    height: 1em;
    padding: 0; 
    border: none; 
  }

  .d-flex.flex-wrap.gap-2.mb-2.p-2.border.rounded-md {
    background-color: #f8f9fa;
    border-color: #e9ecef !important;
    align-items: center;
  }

  .email-tag {
    background-color: #007bff !important;
    color: white;
    font-size: 0.9em;
    white-space: nowrap; 
    display: inline-flex; 
    align-items: center; 
    gap: 0.30rem; 
    padding: 0.4em 0.75em; 
    border-radius: 9999px;
  }

  .autocomplete-suggestions {
    position: absolute;
    top: 100%; 
    max-height: 250px; 
    overflow-y: auto;
    border: 1px solid #dee2e6;
    background-color: white;
  }

  .list-group-item {
    padding: 0.80rem 1.25rem;
    border-bottom: 1px solid rgba(0, 0, 0, 0.125);
    cursor: pointer;
  }

</style>