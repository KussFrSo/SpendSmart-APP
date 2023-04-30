<template>
  <Layout>
    <template #header>
      <Header />
    </template>
    <template #resume>
      <Resume />
    </template>
    <template #movements>
      <Movements />
    </template>
  </Layout>
</template>

<script setup>
import Header from "@/components/Header.vue";
import Layout from "@/components/Layout.vue";
import Resume from "@/components/Resume.vue";
import Movements from "@/components/Movements.vue";
</script>
