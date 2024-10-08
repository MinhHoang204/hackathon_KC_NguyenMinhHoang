<template>
    <div class="modal">
      <div class="modal-content">
        <h2>{{ isEdit ? 'Edit Product' : 'Add New Product' }}</h2>
        <form @submit.prevent="submitForm">
          <div>
            <label for="name">Product Name:</label>
            <input type="text" v-model="product.name" id="name" required />
            <p v-if="errors.name" class="error">{{ errors.name }}</p>
          </div>
          <div>
            <label for="image">Image:</label>
            <input type="text" v-model="product.image" id="image" required />
            <p v-if="errors.image" class="error">{{ errors.image }}</p>
          </div>
          <div>
            <label for="price">Price:</label>
            <input type="number" v-model="product.price" id="price" required />
            <p v-if="errors.price" class="error">{{ errors.price }}</p>
          </div>
          <div>
            <label for="quantity">Quantity:</label>
            <input type="number" v-model="product.quantity" id="quantity" required />
            <p v-if="errors.quantity" class="error">{{ errors.quantity }}</p>
          </div>
          <div class="actions">
            <button type="submit">{{ isEdit ? 'Save' : 'Add' }}</button>
            <button @click="closeForm" type="button">Close</button>
          </div>
        </form>
      </div>
    </div>
</template>
  
<script>
  export default {
    props: {
      isEdit: {
        type: Boolean,
        default: false,
      },
      existingProducts: {
        type: Array,
        required: true,
      },
      currentProduct: {
        type: Object,
        default: () => ({}),
      },
    },
    data() {
      return {
        product: { ...this.currentProduct }, // Clone dữ liệu sản phẩm để chỉnh sửa
        errors: {},
      };
    },
    methods: {
      validateForm() {
        this.errors = {};
  
        // Validate name
        if (!this.product.name) {
          this.errors.name = 'Name cannot be empty';
        } else if (
          this.existingProducts.some(
            (p) => p.name === this.product.name && p.id !== this.product.id
          )
        ) {
          this.errors.name = 'Name already exists';
        }
  
        // Validate image
        if (!this.product.image) {
          this.errors.image = 'Image cannot be empty';
        }
  
        // Validate price
        if (this.product.price < 10000) {
          this.errors.price = 'Price must be at least 10.000 đ';
        }
  
        // Validate quantity
        if (this.product.quantity < 1 || this.product.quantity > 100) {
          this.errors.quantity = 'Quantity must be between 1 and 100';
        }
  
        return Object.keys(this.errors).length === 0;
      },
      submitForm() {
        if (this.validateForm()) {
          this.$emit('save-product', this.product); // Gửi dữ liệu sản phẩm đã chỉnh sửa
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
  