<template>
  <Layout>
    <template #header>
      <Header />
    </template>
    <template #resume>
      <Resume
        totalLabel="Ahorro total"
        :label="label"
        :totalAmount="totalAmount"
        :amount="amount"
      >
        <template #graphic>
          <Graphic :amounts="amounts" @getAmountIndex="getAmountIndex" />
        </template>
        <template #action>
          <Action @createMovement="createMovement" />
        </template>
      </Resume>
    </template>
    <template #movements>
      <Movements :movements="movements" @remove="removeMovement" />
    </template>
  </Layout>
</template>

<script setup>
import { ref, reactive, computed, onMounted } from "vue";
import Header from "@/components/Header.vue";
import Layout from "@/components/Layout.vue";
import Resume from "@/components/Resume/Index.vue";
import Movements from "@/components/Movements/Index.vue";
import Action from "@/components/Action.vue";
import Graphic from "@/components/Resume/Graphic.vue";

const label = ref("Diciembre 31");
const amount = ref(null);
let movements = reactive([]);

onMounted(() => {
  const movementsSaved = JSON.parse(localStorage.getItem("movements"));

  if (Array.isArray(movementsSaved)) {
    movements = movementsSaved.map((x) => {
      return { ...x, time: new Date(x.time) };
    });
  }
});

const amounts = computed(() => {
  const today = new Date();
  const oldDate = new Date(today.getDate() - 30);

  const lastDays = movements
    .filter((x) => {
      return new Date(x.time) > oldDate;
    })
    .map((x) => x.amount);

  return lastDays.map((m, i) => {
    const lastMovements = lastDays.slice(0, i + 1);
    return lastMovements.reduce((suma, movement) => {
      return suma + movement;
    }, 0);
  });
});

const totalAmount = computed(() => {
  return movements.reduce((suma, { amount }) => {
    return suma + amount;
  }, 0);
});

const createMovement = (movement) => {
  movements.push(movement);
  save;
};

const removeMovement = (id) => {
  const index = movements.findIndex((x) => x.id === id);
  movements.splice(index, 1);
  save();
};

const save = () => {
  localStorage.setItem("movements", JSON.stringify(movements));
};

const getAmountIndex = (value) => {
  amount.value = value;
};
</script>
