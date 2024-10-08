<template>
    <div v-if="isVisible" class="modal">
      <div class="modal-content">
        <h2>Add New Product</h2>
        <form @submit.prevent="submitForm">
          <div>
            <label for="name">Product Name:</label>
            <input type="text" v-model="name" id="name" required />
            <p v-if="errors.name" class="error">{{ errors.name }}</p>
          </div>
          <div>
            <label for="image">Image:</label>
            <input type="text" v-model="image" id="image" required />
            <p v-if="errors.image" class="error">{{ errors.image }}</p>
          </div>
          <div>
            <label for="price">Price:</label>
            <input type="number" v-model="price" id="price" required />
            <p v-if="errors.price" class="error">{{ errors.price }}</p>
          </div>
          <div>
            <label for="quantity">Quantity:</label>
            <input type="number" v-model="quantity" id="quantity" required />
            <p v-if="errors.quantity" class="error">{{ errors.quantity }}</p>
          </div>
          <div class="actions">
            <button type="submit">Add Product</button>
            <button @click="closeForm" type="button">Close</button>
          </div>
        </form>
      </div>
    </div>
</template>
  
<script>
  export default {
    props: {
      isVisible: {
        type: Boolean,
        required: true,
      },
      existingProducts: {
        type: Array,
        required: true,
      },
    },
    data() {
      return {
        name: '',
        image: '',
        price: '',
        quantity: '',
        errors: {},
      };
    },
    methods: {
      validateForm() {
        this.errors = {};
  
        // Validate name
        if (!this.name) {
          this.errors.name = 'Name cannot be empty';
        } else if (this.existingProducts.some((product) => product.name === this.name)) {
          this.errors.name = 'Name already exists';
        }
  
        // Validate image
        if (!this.image) {
          this.errors.image = 'Image cannot be empty';
        }
  
        // Validate price
        if (this.price < 10000) {
          this.errors.price = 'Price must be at least 10.000 đ';
        }
  
        // Validate quantity
        if (this.quantity < 1 || this.quantity > 100) {
          this.errors.quantity = 'Quantity must be between 1 and 100';
        }
  
        // Return true if no errors
        return Object.keys(this.errors).length === 0;
      },
      submitForm() {
        if (this.validateForm()) {
          const newProduct = {
            name: this.name,
            image: this.image,
            price: this.price,
            quantity: this.quantity,
          };
          this.$emit('add-product', newProduct);
          this.closeForm();
        }
      },
      closeForm() {
        this.$emit('close-form');
      },
    },
  };
</script>
  
<style scoped>
  .error {
    color: red;
  }
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
    width: 400px;
  }
  
  .actions {
    margin-top: 10px;
  }
</style>
  