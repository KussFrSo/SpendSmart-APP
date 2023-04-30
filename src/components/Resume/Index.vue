<template>
  <main>
    <p>{{ labelVisual }}</p>
    <h1>{{ amountCurrency }}</h1>
    <div class="graphic">
      <slot name="graphic"></slot>
    </div>
    <div class="action">
      <slot name="action"></slot>
    </div>
  </main>
</template>

<script setup>
import { defineProps, toRefs, computed } from "vue";
const props = defineProps({
  totalLabel: {
    type: String,
    default: null,
  },
  label: String,
  totalAmount: Number,
  amount: {
    type: Number,
    default: null,
  },
});

const { totalLabel, label, amount, totalAmount } = toRefs(props);

const currencyFormater = new Intl.NumberFormat("es-ES", {
  style: "currency",
  currency: "EUR",
});

const labelVisual = computed(() => {
  return label.value ? label.value : totalLabel.value;
});

const amountVisual = computed(() => {
  return amount.value !== null ? amount.value : totalAmount.value;
});

const amountCurrency = computed(() => {
  return currencyFormater.format(amountVisual.value);
});
</script>

<style scoped>
main {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  width: 100%;
}
h1,
p {
  margin: 0;
  text-align: center;
}
h1 {
  margin-top: 14px;
  color: var(--brand-green);
}
.graphic {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  padding: 48px 24px;
  box-sizing: border-box;
}
</style>
