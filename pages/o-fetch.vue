<template>
    <h1 class="text-white font-bold py-16 text-center text-3xl">O Fetch</h1>
    <div v-if="pending">Carregando</div>
    <div v-else-if="error">Error: {{ error.message }}</div>
    <pre class="text-white text-center" v-else>
				{{ userProfile }}
		</pre>
    <div class="flex justify-center">
    <button class="inline-block rounded bg-emerald-500 px-5 py-3 text-sm font-medium text-white"
      @click="fetchUserProducts">
      Carregar Produtos
    </button>
  </div>
    <div class="text-emerald-500 my-6 text-center" v-if="isLoadingProducts">
      Carregando Produtos
    </div>
    <div v-else-if="userProducts.length > 0">
      <ul class="py-5">
        <li class="text-white text-center" v-for="product in userProducts" :key="product.id">
          {{ product.name }} - {{ product.price }}
        </li>
      </ul>
    </div>
    <back-home />
</template>

<script setup lang="ts">
import type { IUserProduct } from "~/services/user/types";
const { $http } = useNuxtApp();
const {
  data: userProfile,
  pending,
  error,
} = await useAsyncData("user-profile", () => $http.user.getUserProfile());
const isLoadingProducts = ref(false);
const userProducts = ref(<IUserProduct[]>[]);
const fetchUserProducts = async () => {
  try {
    isLoadingProducts.value = true;
    userProducts.value = await $http.user.getUserProducts();
  } catch (error: any) {
    alert(error.message);
  } finally {
    isLoadingProducts.value = false;
  }
};
</script>
