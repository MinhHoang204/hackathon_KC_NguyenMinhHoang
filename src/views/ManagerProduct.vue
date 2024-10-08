<template>
    <div>
      <h1>Manager Product</h1>
      <button @click="openAddForm">Add New Product</button>
      
      <!-- Ô tìm kiếm sản phẩm -->
      <input 
        type="text" 
        v-model="searchQuery" 
        placeholder="Search by name"
        @input="filterProducts" 
      />
  
      <ProductTable
        :products="filteredProducts"
        @edit-product="openEditForm"
        @delete-product="openDeleteModal"
      />
      <ProductForm
        v-if="isFormVisible"
        :existingProducts="products"
        :currentProduct="currentProduct"
        :isEdit="isEdit"
        @close-form="closeForm"
        @save-product="saveProduct"
      />
  
      <DeleteModal
        v-if="isDeleteModalVisible"
        @confirm-delete="deleteProduct"
        @cancel-delete="closeDeleteModal"
      />
    </div>
</template>
  
<script>
  import ProductTable from './ProductTable.vue';
  import ProductForm from './ProductForm.vue';
  import DeleteModal from './DeleteModal.vue';
  
  export default {
    components: {
      ProductTable,
      ProductForm,
      DeleteModal,
    },
    data() {
      return {
        isFormVisible: false,
        isDeleteModalVisible: false,
        products: this.getProductsFromLocalStorage(),
        filteredProducts: this.getProductsFromLocalStorage(), // Biến lưu trữ sản phẩm sau khi lọc
        currentProduct: null,
        isEdit: false,
        searchQuery: '', // Biến lưu trữ giá trị ô tìm kiếm
      };
    },
    methods: {
      openAddForm() {
        this.currentProduct = null;
        this.isEdit = false;
        this.isFormVisible = true;
      },
      openEditForm(product) {
        this.currentProduct = { ...product };
        this.isEdit = true;
        this.isFormVisible = true;
        this.$nextTick(() => {
          document.querySelector('#product-name-input').focus(); // Focus vào input đầu tiên khi mở form
        });
      },
      openDeleteModal(product) {
        this.currentProduct = { ...product };
        this.isDeleteModalVisible = true;
      },
      deleteProduct() {
        this.products = this.products.filter(
          (product) => product.id !== this.currentProduct.id
        );
        this.saveProductsToLocalStorage();
        this.isDeleteModalVisible = false;
        this.filterProducts();
      },
      closeDeleteModal() {
        this.isDeleteModalVisible = false;
      },
      saveProduct(updatedProduct) {
        if (this.isEdit) {
          const index = this.products.findIndex(
            (product) => product.id === updatedProduct.id
          );
          if (index !== -1) {
            this.products.splice(index, 1, updatedProduct);
          }
        } else {
          this.products.push({ ...updatedProduct, id: this.products.length + 1 });
        }
        this.saveProductsToLocalStorage();
        this.isFormVisible = false;
        this.filterProducts();
      },
      closeForm() {
        this.isFormVisible = false;
      },
      filterProducts() {
        this.filteredProducts = this.products.filter((product) =>
          product.name.toLowerCase().includes(this.searchQuery.toLowerCase())
        );
      },
      getProductsFromLocalStorage() {
        const products = localStorage.getItem('products');
        return products ? JSON.parse(products) : [];
      },
      saveProductsToLocalStorage() {
        localStorage.setItem('products', JSON.stringify(this.products));
      },
    },
  };
</script>
  