<template>
  <div>
    
    <!-- Button to open the Modal  -->
    <button @click="openModal" class="btn btn-info rounded-md">
      Send Email
    </button>

    <div v-if="isModalOpen" class="modal d-block" tabindex="-1" role="dialog" >
      <div class="modal-dialog modal-dialog-centered modal-lg" role="document">
        <div class="modal-content rounded-lg">
          
          <div class="modal-header">
           
            <h5 class="modal-title" id="emailModalLabel">Email Form</h5>
            
          </div>
          <div class="modal-body">
              <!-- TODO: Add the form here -->

              Subject: <br>

              Description: <br>  
          </div>
         
        </div>
      </div>
    </div>

      <div v-if="isModalOpen" class="modal-backdrop show"></div>
  
  </div>
</template>

<script setup>
  
  import { ref, onMounted, onBeforeUnmount, nextTick } from 'vue';

  const isModalOpen = ref(false);

  // Opens Modal
  const openModal = async () => {
    isModalOpen.value = true;
  };

</script>

<style scoped>


</style>