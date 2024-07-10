<template>
  <q-layout view="hHh lpR fFf">
    <q-header elevated>
      <q-toolbar>
        <q-btn flat dense round icon="menu" aria-label="Menu" @click="toggleLeftDrawer" />
        <q-toolbar-title>Product CRUD Application</q-toolbar-title>
        <q-btn flat dense round icon="add" aria-label="Add Product" @click="navigateToCreate" />
      </q-toolbar>
    </q-header>

    <q-drawer v-model="leftDrawerOpen" show-if-above bordered>
      <q-list>
        <q-item clickable v-ripple @click="navigateToHome">
          <q-item-section>
            <q-item-label>Home</q-item-label>
          </q-item-section>
        </q-item>
        <q-item clickable v-ripple @click="navigateToCreate">
          <q-item-section>
            <q-item-label>Create Product</q-item-label>
          </q-item-section>
        </q-item>
      </q-list>
    </q-drawer>

    <q-page-container>
      <router-view :products="products" @update-products="updateProducts" />
    </q-page-container>

  </q-layout>
</template>

<script>
export default {
  data() {
    return {
      leftDrawerOpen: false,
      products: []
    }
  },
  methods: {
    toggleLeftDrawer() {
      this.leftDrawerOpen = !this.leftDrawerOpen;
    },
    navigateToHome() {
      this.$router.push('/');
    },
    navigateToCreate() {
      this.$router.push('/create');
    },
    navigateToUpdate() {
      this.$router.push('/update');
    },
    updateProducts(products) {
      this.products = products;
    }
  },
  mounted() {
    this.products = JSON.parse(localStorage.getItem('products')) || [];
  }
}
</script>

<style scoped>
.q-img {
  height: 150px;
  width: 150px;
  border-radius: 10px;
}
</style>