<template>
  <div class="mb-3">
    
    <label v-if="label">
      {{ label }}
    </label>
    
    <input
      :id="id"
      :type="type"
      :value="value"
      :placeholder="placeholder"
      :required="required"
      @input="handleInput"
      class="form-control rounded-md"
      :class="{ 'is-invalid': error }"
      ref="inputElement"
    />

    <!-- Display error message -->
    <div v-if="error" class="invalid-feedback d-block">{{ error }}</div>

  </div>
</template>

<script setup>
  import { ref } from 'vue';

  // Component Props
  const props = defineProps({
    
    id: { 
      type: String,
      required: true
    },

    label: { 
      type: String,
      default: ''
    },

    value: { 
      type: String,
      default: ''
    },

    placeholder: { 
      type: String,
      default: ''
    },

    type: { 
      type: String,
      default: 'text'
    },

    required: { 
      type: Boolean,
      default: false
    },

    error: { 
      type: String,
      default: ''
    }
  });

  const inputElement = ref(null);

</script>