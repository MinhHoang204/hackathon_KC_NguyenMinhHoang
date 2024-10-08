<template>
    <div class="modal">
      <div class="modal-content">
        <h2>Confirm Delete</h2>
        <p>Are you sure you want to delete this product?</p>
        <div class="actions">
          <button @click="$emit('confirm-delete')">Delete</button>
          <button @click="$emit('cancel-delete')">Cancel</button>
        </div>
      </div>
    </div>
</template>
  
<script>
  export default {
    name: 'DeleteModal',
  };
</script>
  
  <style scoped>
  .modal {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
  }
  
  .modal-content {
    background-color: white;
    padding: 20px;
    border-radius: 5px;
    width: 300px;
  }
  
  .actions {
    margin-top: 10px;
    display: flex;
    justify-content: space-between;
  }
</style>
  