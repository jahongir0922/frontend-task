<script setup lang="ts">
import UserManagement from "./components/UserManagement.vue";
import ProductManagement from "./components/ProductManagement.vue";
import AnalyticsDashboard from "./components/AnalyticsDashboard.vue";
</script>

<template>
  <div class="min-h-screen bg-gray-50">
    <div class="container mx-auto p-6">
      <UserManagement />
    </div>
    <div class="container mx-auto p-6">
      <ProductManagement />
    </div>
    <div class="container mx-auto p-6">
      <AnalyticsDashboard />
    </div>
  </div>
</template>
