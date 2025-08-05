<script setup lang="ts">
import { createColumnHelper } from "@tanstack/vue-table";
import Button from "./ui/Button.vue";
import { ref, reactive, computed, h } from "vue";
import DataTable from "./DataTable.vue";
import ConfirmActionDialog from "./ConfirmActionDialog.vue";
import Input from "./ui/Input.vue";
import Label from "./ui/Label.vue";
interface Product {
  id: number;
  name: string;
  category: string;
  price: number;
  stockQuantity: number;
  status: "in_stock" | "out_of_stock" | "discontinued";
  createdAt?: string;
}
// Data
const products = ref<Product[]>([
  {
    id: 1,
    name: "an Product",
    category: "an Category",
    price: 5,
    stockQuantity: 32,
    status: "in_stock",
    createdAt: "05.08.2025",
  },
  {
    id: 2,
    name: "an Product 2",
    category: "an Category2 ",
    price: 46,
    stockQuantity: 5,
    status: "out_of_stock",
    createdAt: "05.08.2025",
  },
  {
    id: 3,
    name: "an Product 3",
    category: "an Category3",
    price: 77,
    stockQuantity: 26,
    status: "discontinued",
    createdAt: "05.08.2025",
  },
  {
    id: 4,
    name: "an Product 4",
    category: "an Category4",
    price: 25,
    stockQuantity: 32,
    status: "in_stock",
    createdAt: "05.08.2025",
  },
]);
const showDeleteDialog = ref(false);
const productToDelete = ref<Product | null>(null);
const editingProduct = ref<Product | null>(null);
const showProductForm = ref(false);

const loading = ref(false);

// Computed
const isEditing = computed(() => editingProduct.value !== null);
// Form data
const productForm = reactive({
  name: "",
  category: "",
  price: 0 as number,
  stockQuantity: 0 as number,
  status: "in_stock" as Product["status"],
});
// const handleQuantity = (event: Event) => {
//   const value = parseFloat((event.target as HTMLInputElement).value);
//   if (!isNaN(value)) {
//     productForm.stockQuantity = value;
//   }
// };
const columnHelper = createColumnHelper<Product>();
const columns = [
  columnHelper.accessor("name", {
    header: "Product Name",
    cell: ({ getValue }) => getValue(),
  }),
  columnHelper.accessor("category", {
    header: "Category",
    cell: ({ getValue }) => getValue(),
  }),
  columnHelper.accessor("price", {
    header: "Price",
    cell: ({ getValue }) => getValue(),
  }),
  columnHelper.accessor("stockQuantity", {
    header: "Stock Quantity",
    // cell: ({ getValue }) => getValue(),
    cell: ({ getValue }) => {
      const role = getValue();

      return h(
        "span",
        {
          class: `inline-flex px-2 py-1 text-xs font-semibold rounded-full ${
            role < 10 ? "text-red-800 bg-red-100" : ""
          }`,
        },
        role
      );
    },
  }),
  columnHelper.accessor("status", {
    header: "Status",
    cell: ({ getValue }) => {
      const role = getValue();
      const roleColors = {
        discontinued: "bg-red-100 text-red-800",
        out_of_stock: "bg-blue-100 text-blue-800",
        in_stock: "bg-green-100 text-green-800",
      };
      return h(
        "span",
        {
          class: `inline-flex px-2 py-1 text-xs font-semibold rounded-full ${roleColors[role]}`,
        },
        role.charAt(0).toUpperCase() + role.slice(1)
      );
    },
  }),
  columnHelper.display({
    id: "actions",
    header: "Actions",
    cell: ({ row: { original } }) => {
      return h("div", { class: "flex gap-2" }, [
        h(
          Button,
          {
            variant: "outline",
            size: "sm",
            onClick: () => editProduct(original),
          },
          () => "Edit"
        ),
        h(
          Button,
          {
            variant: "destructive",
            size: "sm",
            onClick: () => confirmDelete(original),
          },
          () => "Delete"
        ),
      ]);
    },
  }),
];
const editProduct = (product: Product) => {
  console.log(product);
  editingProduct.value = product;
  productForm.name = product.name;
  productForm.category = product.category;
  productForm.price = product.price;
  productForm.stockQuantity = product.stockQuantity;
  productForm.status = product.status;
  showProductForm.value = true;
};
const cancelForm = () => {
  showProductForm.value = false;
  editingProduct.value = null;
  //   resetForm()
};
const confirmDelete = (product: Product) => {
  productToDelete.value = product;
  showDeleteDialog.value = true;
};
const deleteProduct = () => {
  if (productToDelete.value) {
    products.value = products.value.filter(
      (u) => u.id !== productToDelete.value!.id
    );
    productToDelete.value = null;
    showDeleteDialog.value = false;
  }
};
const cancelDelete = () => {
  productToDelete.value = null;
  showDeleteDialog.value = false;
};
const saveProduct = () => {
  if (!productForm.name || !productForm.price) return;

  loading.value = true;

  // Simulate API call
  setTimeout(() => {
    if (isEditing.value && editingProduct.value) {
      // Update existing product
      const index = products.value.findIndex(
        (u) => u.id === editingProduct.value!.id
      );
      if (index !== -1) {
        // console.log("object");
        products.value[index] = {
          ...editingProduct.value,
          name: productForm.name,
          category: productForm.category,
          price: productForm.price,
          stockQuantity: productForm.stockQuantity,
          status: productForm.status,
        };

        // console.log(products.value[index]);
      }
    }

    loading.value = false;
    showProductForm.value = false;
    editingProduct.value = null;
  }, 1000);
};
</script>
<template>
  <div>
    <!-- Product Table -->
    <div class="bg-white rounded-lg shadow">
      <DataTable :data="products" :columns="columns" :loading="false" />
    </div>
    <!-- Product Form Modal -->
    <div
      v-if="showProductForm"
      class="fixed inset-0 z-50 flex items-center justify-center bg-black bg-opacity-50"
    >
      <div class="bg-white rounded-lg p-6 w-full max-w-md mx-4">
        <h2 class="text-xl font-semibold mb-4">"Edit Product"</h2>

        <form @submit.prevent="saveProduct" class="space-y-4">
          <div>
            <Label for="name">Name</Label>
            <Input
              id="name"
              v-model="productForm.name"
              placeholder="Enter name"
              required
              class="mt-1"
            />
          </div>

          <div>
            <Label for="category">Category</Label>
            <Input
              id="category"
              v-model="productForm.category"
              type="text"
              placeholder="category"
              class="mt-1"
            />
          </div>

          <div>
            <Label for="stockQuantity">Stock Quantity</Label>
            <Input
              id="stockQuantity"
              type="number"
              v-model="productForm.price"
              placeholder="Quantity"
              required
              class="mt-1"
            />
          </div>

          <div>
            <Label for="status">Status</Label>
            <select
              id="status"
              v-model="productForm.status"
              class="mt-1 flex h-10 w-full rounded-md border border-input bg-background px-3 py-2 text-sm ring-offset-background focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2"
            >
              <option value="in_stock">In stock</option>
              <option value="out_of_stock">Out of stock</option>
              <option value="discontinued">Discontinued</option>
            </select>
          </div>

          <div class="flex gap-3 pt-4">
            <Button
              type="submit"
              :disabled="loading"
              class="flex-1 bg-blue-600 hover:bg-blue-700"
            >
              {{ loading ? "Saving..." : "Update Product" }}
            </Button>
            <Button
              type="button"
              variant="outline"
              @click="cancelForm"
              class="flex-1"
            >
              Cancel
            </Button>
          </div>
        </form>
      </div>
    </div>
    <ConfirmActionDialog
      v-model:open="showDeleteDialog"
      title="Delete Product"
      :description="`Are you sure you want to delete ${productToDelete?.name}? This action cannot be undone.`"
      action-variant="destructive"
      @continue-action="deleteProduct"
      @cancel-action="cancelDelete"
    />
  </div>
</template>
