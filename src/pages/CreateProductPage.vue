<template>
  <div class="q-pa-md">
    <q-form @submit.prevent="createProduct">
      <q-input v-model="newProduct.name" label="Name" outlined class="q-mb-md" />
      <q-input v-model="newProduct.price" label="Price" type="number" outlined class="q-mb-md" />
      <q-input v-model="newProduct.description" label="Description" type="textarea" outlined class="q-mb-md" />

      <q-uploader
        label="Upload Image"
        accept="image/*"
        @added="onImageAdded"
        class="q-mb-md"
        bordered
      />

      <q-btn label="Create Product" type="submit" color="primary" />
    </q-form>

    <!-- Success Dialog -->
    <q-dialog v-model="showSuccessDialog" persistent>
      <q-card class="q-pa-md" style="min-width: 350px">
        <q-card-section>
          <div class="text-h6">Success</div>
        </q-card-section>

        <q-card-section>
          <div>Product Added Successfully</div>
        </q-card-section>

        <q-card-actions align="right">
          <q-btn flat round color="primary" label="OK" @click="navigateToProductPage" />
        </q-card-actions>
      </q-card>
    </q-dialog>
  </div>
</template>

<script>
import products from '../components/products'; // Import the existing products array

export default {
  data() {
    return {
      newProduct: {
        name: '',
        price: '',
        description: '',
        image: null,
        date: null
      },
      showSuccessDialog: false
    };
  },

  methods: {
    createProduct() {
      if (!this.newProduct.image) {
        alert('Please upload an image');
        return;
      }

      const reader = new FileReader();
      reader.onload = () => {
        const imageData = reader.result;
        const currentDate = Date.now();
        const newProductData = {
          id: currentDate, // Generate a unique ID for the new product using Date.now()
          name: this.newProduct.name,
          price: this.newProduct.price,
          description: this.newProduct.description,
          image: imageData,
          date: currentDate // Set the date as the current timestamp
        };

        products.push(newProductData); // Add the new product to the existing products array

        // Reset the form
        this.newProduct = {
          name: '',
          price: '',
          description: '',
          image: null,
          date: null
        };

        // Show success dialog
        this.showSuccessDialog = true;
      };
      reader.readAsDataURL(this.newProduct.image);
    },

    onImageAdded(files) {
      if (files.length > 0) {
        this.newProduct.image = files[0];
      }
    },

    navigateToProductPage() {
      this.showSuccessDialog = false;
      this.$router.push('/product-page'); // Navigate to the ProductPage
    }
  }
};
</script>

<style lang="css">
.q-mb-md {
  margin-bottom: 16px;
}
</style>
