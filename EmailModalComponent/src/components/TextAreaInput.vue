<template>
  <div class="mb-3">
    
    <!-- Display the label -->
    <label v-if="label">
      {{ label }}
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


