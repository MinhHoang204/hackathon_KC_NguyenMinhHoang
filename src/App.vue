<template>
  <div>
    <nav>
      <router-link to="/admin" class="nav-link">Dashboard</router-link>
      <router-link to="/admin/manager-product" class="nav-link">Manager Product</router-link>
      <router-link to="/admin/manager-category" class="nav-link">Manager Category</router-link>
    </nav>
    <router-view></router-view>
    <ManagerProduct></ManagerProduct>
    <ManagerCategory></ManagerCategory>
    <AddProductForm></AddProductForm>
  </div>
</template>

<script setup>
 import ManagerProduct from './views/ManagerProduct.vue';
 import ManagerCategory from './views/ManagerCategory.vue';
 import AddProductForm from './views/AddProductForm.vue';
</script>

<style scoped>
nav {
  background-color: #333;
  padding: 1rem;
}

.nav-link {
  color: white;
  margin-right: 10px;
  text-decoration: none;
  padding: 5px 10px;
}

.router-link-active {
  background-color: #007bff;
  font-weight: bold;
  border-radius: 5px;
}
</style>
