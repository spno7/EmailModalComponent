<template>
  <div>
    
    <div style="display: flex; justify-content: center; align-items: center; height: 30vh;">
      <!-- Button to open the Modal  -->
      <button @click="openModal" class="btn btn-info rounded-md">
        Send Email
      </button>
    </div>

    <div v-if="isModalOpen" class="modal d-block" tabindex="-1" role="dialog" >
      <div class="modal-dialog modal-dialog-centered modal-lg" role="document">
        <div class="modal-content rounded-lg">
          
          <div class="modal-header">
           
            <h5 class="modal-title" id="emailModalLabel"> Email Form </h5>
            <button type="button" class="btn-close" aria-label="Close" @click="closeModal"> 
            </button>

          </div>
          
          <div class="modal-body">
            
            <EmailForm />

          </div>
         
        </div>
      </div>
    </div>

    <div v-if="isModalOpen" class="modal-backdrop show"> </div>
  
  </div>
</template>

<script setup>
  
  import { ref } from 'vue';
  import EmailForm from './EmailForm.vue'

  const isModalOpen = ref(false);

  // Opens Modal
  const openModal = async () => {
    isModalOpen.value = true;
  };

  // Close Modal
  const closeModal = () => {
    isModalOpen.value = false;
  }

</script>

<style scoped>

  .btn-close {

    background-image: url("data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 16 16' fill='%23fff'%3e%3cpath d='M.293.293a1 1 0 011.414 0L8 6.586 14.293.293a1 1 0 111.414 1.414L9.414 8l6.293 6.293a1 1 0 01-1.414 1.414L8 9.414l-6.293 6.293a1 1 0 01-1.414-1.414L6.586 8 .293 1.707a1 1 0 010-1.414z'/%3e%3c/svg%3e");

    background-color: red;

    opacity: 0.7;
  }

</style>