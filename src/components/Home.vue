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
        <template #graphic> <Graphic :amounts="amounts" /> </template>
        <template #action> <Action /> </template>
      </Resume>
    </template>
    <template #movements>
      <Movements :movements="movements" />
    </template>
  </Layout>
</template>

<script setup>
import { ref, reactive } from "vue";
import Header from "@/components/Header.vue";
import Layout from "@/components/Layout.vue";
import Resume from "@/components/Resume/Index.vue";
import Movements from "@/components/Movements/Index.vue";
import Action from "@/components/Action.vue";
import Graphic from "@/components/Resume/Graphic.vue";

const label = ref("Diciembre 31");
const amount = ref(null);
const amounts = [100, 200, 500, 200, -400, -600, -300, 0, 300, 500];
const movements = reactive([
  {
    id: 1,
    title: "Movimiento",
    description: "Deposito de salario",
    amount: 1000,
  },
  {
    id: 2,
    title: "Movimiento 1",
    description: "Deposito de honorarios",
    amount: 500,
  },
  {
    id: 3,
    title: "Movimiento 3",
    description: "Comida",
    amount: -100,
  },
  {
    id: 4,
    title: "Movimiento 4",
    description: "Colegiatura",
    amount: 1000,
  },
  {
    id: 5,
    title: "Movimiento 5",
    description: "Reparación equipo",
    amount: 1000,
  },
]);
</script>
