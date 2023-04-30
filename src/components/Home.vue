<template>
  <Layout>
    <template #header>
      <Header />
    </template>
    <template #resume>
      <Resume
        totalLabel="Ahorro total"
        :label="label"
        totalAmount="1000000"
        :amount="amount"
      >
        <template #graphic> graphic </template>
        <template #action> </template>
      </Resume>
    </template>
    <template #movements>
      <Movements />
    </template>
  </Layout>
</template>

<script setup>
import { ref } from "vue";
import Header from "@/components/Header.vue";
import Layout from "@/components/Layout.vue";
import Resume from "@/components/Resume.vue";
import Movements from "@/components/Movements.vue";

const label = ref("Diciembre 31");
const amount = ref(null);
</script>
