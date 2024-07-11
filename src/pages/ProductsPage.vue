<template>
  <div class="q-pa-md">
    <div class="row q-col-gutter-md">
      <div class="col-4" v-for="(product, index) in products" :key="index">
        <q-card class="my-card">
          <div class="box">
            <q-img :src="product.image" class="image"/> 
          </div>
          <q-card-section>
            <div class="text-h6">{{ product.name }}</div>
            <div class="text-subtitle2">XAF{{ formatPrice(product.price) }}</div>
            <div class="text-subtitle2">{{ formatDate(product.date) }}</div>
          </q-card-section>

          <q-card-section>
            {{ product.description }}
          </q-card-section>

          <q-card-actions align="right">
            <q-btn flat round color="primary" icon="edit" @click="showUpdateForm(product)" />
            <q-btn flat round color="negative" icon="delete" @click="deleteProduct(product)" />
          </q-card-actions>
        </q-card>
      </div>
    </div>

    <!-- Update Form Popup -->
    <q-dialog v-model="showUpdateFormDialog" persistent class="dialog-size">
      <q-card class="q-pa-md q-gutter-md" style="min-width: 60rem">
        <q-card-section>
          <div class="text-h6">Update Product</div>
        </q-card-section>

        <q-card-section>
          <q-form @submit.prevent="updateProduct" class="q-gutter-md q-display-flex q-flex-column">
            <div class="flex-row">
              <div class="q-col">
                <q-input v-model="updatedProduct.name" label="Name" outlined class="q-mb-md" />
                <q-input v-model="updatedProduct.price" label="Price" type="number" outlined class="q-mb-md" />
                <q-input v-model="updatedProduct.description" label="Description" type="textarea" outlined class="q-mb-md" />
              </div>
              <div class="q-col">
                <q-uploader
                  label="Upload Image"
                  accept="image/*"
                  @added="onImageAdded"
                  class="q-mb-md"
                  bordered
                />
              </div>
            </div>
            <div class="q-mt-md q-display-flex q-justify-between q-width-100">
              <q-btn label="Cancel" flat color="negative" @click="showUpdateFormDialog = false" />
              <q-btn label="Update" type="submit" color="primary" />
            </div>
          </q-form>
        </q-card-section>
      </q-card>
    </q-dialog>

    <!-- Success Dialog -->
    <q-dialog v-model="showSuccessDialog" persistent>
      <q-card class="q-pa-md" style="min-width: 350px">
        <q-card-section>
          <div class="text-h6">Success</div>
        </q-card-section>

        <q-card-section>
          <div>Product Updated Successfully</div>
        </q-card-section>

        <q-card-actions align="right">
          <q-btn flat round color="primary" label="OK" @click="showSuccessDialog = false" />
        </q-card-actions>
      </q-card>
    </q-dialog>
  </div>
</template>

<script>
import products from '../components/products.js';

export default {
  data() {
    return {
      products: [...products], // create a copy of the products array
      showUpdateFormDialog: false,
      showSuccessDialog: false,
      updatedProduct: {},
      newImage: null // store new image file
    };
  },

  methods: {
    showUpdateForm(product) {
      this.updatedProduct = { ...product }; // create a copy of the product object
      this.newImage = null; // reset new image
      this.showUpdateFormDialog = true;
    },

    updateProduct() {
      const index = this.products.findIndex(p => p.id === this.updatedProduct.id);
      if (index !== -1) {
        if (this.newImage) {
          // if a new image is uploaded, use it
          const reader = new FileReader();
          reader.onload = (e) => {
            this.updatedProduct.image = e.target.result;
            this.products[index] = { ...this.updatedProduct, date: Date.now() }; // update the product object with new date
            this.showUpdateFormDialog = false;
            this.showSuccessDialog = true; // show success dialog
          };
          reader.readAsDataURL(this.newImage);
        } else {
          this.products[index] = { ...this.updatedProduct, date: Date.now() }; // update the product object with new date
          this.showUpdateFormDialog = false;
          this.showSuccessDialog = true; // show success dialog
        }
      }
    },

    deleteProduct(product) {
      const index = this.products.findIndex(p => p.id === product.id);
      if (index !== -1) {
        this.products.splice(index, 1);
      }
    },

    onImageAdded(files) {
      this.newImage = files[0]; // store the new image file
    },

    formatDate(timestamp) {
      const date = new Date(timestamp);
      return date.toLocaleDateString(); // Format the date to a readable format
    },

    formatPrice(price) {
      return new Intl.NumberFormat('en-US', {
        style: 'decimal',
        minimumFractionDigits: 0
      }).format(price);
    }
  }
};
</script>

<style lang="css">
.image {
  width: 20rem;
  height: auto;
}
.my-card {
  justify-content: center;
  padding-left: 4rem;
  padding-top: 2rem;
}
.q-flex-1 {
  flex: 1 1 30%;
}
.q-width-100 {
  width: 100%;
}
.flex-row {
  display: flex;
  gap: 1rem;
}
.q-col {
  flex: 1;
}
</style>
