<template>
  <div class="mb-3">
    
    <!-- Display field label -->
    <label v-if="label">
      <b> {{ label }} </b>
    </label>

    <textarea
      :id="id"
      :value="value"
      :placeholder="placeholder"
      :required="required"
      :rows="rows"
      @input="handleInput"
      class="form-control rounded-md"
      :class="{ 'is-invalid': error }"
    />
  
     <!-- Display error message -->
    <div v-if="error" class="invalid-feedback d-block"> {{ error }} </div>

  </div>
</template>

<script setup>

  // Define props that this component accepts
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

    required: { 
      type: Boolean,
      default: false
    },

    rows: { 
      type: [String, Number],
      default: 5
    },

    error: { 
      type: String,
      default: ''
    },

    showCharCount: { 
      type: Boolean,
      default: false
    },
    
    maxLength: {
      type: [String, Number],
      default: 1000
    }
  });


</script>


