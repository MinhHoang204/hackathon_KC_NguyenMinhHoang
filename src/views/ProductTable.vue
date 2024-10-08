<template>
    <table>
      <thead>
        <tr>
          <th>STT</th>
          <th>Image</th>
          <th>Name</th>
          <th>Price</th>
          <th>Quantity</th>
          <th>Option</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(product, index) in products" :key="product.id">
          <td>{{ index + 1 }}</td>
          <td><img :src="product.image" alt="" width="50" /></td>
          <td>{{ product.name }}</td>
          <td>{{ product.price }}</td>
          <td>{{ product.quantity }}</td>
          <td>
            <button @click="editProduct(product.id)">Edit</button>
            <button @click="deleteProduct(product.id)">Delete</button>
          </td>
        </tr>
      </tbody>
    </table>
</template>
  
<script>
  export default {
    props: {
      products: {
        type: Array,
        required: true,
      },
    },
    methods: {
      editProduct(id) {
        console.log("Editing product", id);
      },
      deleteProduct(id) {
        console.log("Deleting product", id);
      },
    },
  };
</script>
  
<style scoped>
  table {
    width: 100%;
    border-collapse: collapse;
  }
  
  th, td {
    border: 1px solid #ddd;
    padding: 8px;
    text-align: center;
  }
  
  button {
    margin-right: 5px;
  }
</style>
  