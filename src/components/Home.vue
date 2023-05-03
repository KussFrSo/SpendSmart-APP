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
import { ref, reactive, computed } from "vue";
import Header from "@/components/Header.vue";
import Layout from "@/components/Layout.vue";
import Resume from "@/components/Resume/Index.vue";
import Movements from "@/components/Movements/Index.vue";
import Action from "@/components/Action.vue";
import Graphic from "@/components/Resume/Graphic.vue";

const label = ref("Diciembre 31");
const amount = ref(null);
const movements = reactive([
  {
    id: 1,
    title: "Movimiento",
    description: "Deposito de salario",
    amount: 1000,
    time: new Date("03/05/2023"),
  },
  {
    id: 2,
    title: "Movimiento 1",
    description: "Deposito de honorarios",
    amount: 500,
    time: new Date("03/05/2023"),
  },
  {
    id: 3,
    title: "Movimiento 3",
    description: "Comida",
    amount: -100,
    time: new Date("02/05/2023"),
  },
  {
    id: 4,
    title: "Movimiento 4",
    description: "Colegiatura",
    amount: 1000,
    time: new Date("01/05/2023"),
  },
  {
    id: 5,
    title: "Movimiento 5",
    description: "Reparación equipo",
    amount: 1000,
    time: new Date("01/05/2023"),
  },
]);

const amounts = computed(() => {
  const today = new Date();
  const oldDate = new Date(today.getDate() - 30);

  const pp = movements
    .filter((x) => {
      return new Date(x.time) > oldDate;
    })
    .map((x) => x.amount);

  return pp;
});
</script>
