<template>
  <div>
    <svg
      @touchstart="tap"
      @touchmove="tap"
      @touchend="untap"
      :viewBox="`0 0 ${GRAPHIC_WIDTH} ${GRAPHIC_HEIGHT}`"
    >
      <line
        stroke="#c4c4c4"
        stroke-width="2"
        x1="0"
        :x2="GRAPHIC_WIDTH"
        :y1="lineZero"
        :y2="lineZero"
      />
      <polyline
        fill="none"
        stroke="#0689B0"
        stroke-width="2"
        :points="points"
      />
      <line
        v-show="showPointer"
        stroke="#04b500"
        stroke-width="2"
        :x1="pointer"
        :x2="pointer"
        y1="0"
        :y2="GRAPHIC_HEIGHT"
      />
    </svg>
    <p>Últimos 30 días</p>
  </div>
</template>

<script setup>
import { defineProps, toRefs, computed, ref, watch, defineEmits } from "vue";

const GRAPHIC_HEIGHT = 200;
const GRAPHIC_WIDTH = 300;

const { amounts } = toRefs(props);
const showPointer = ref(false);
const pointer = ref(0);

watch(pointer, (value) => {
  const index = Math.ceil(value / (GRAPHIC_WIDTH / amounts.value.length));
  if (index < 0 || index > amounts.value.length) return;
  emit("getAmountIndex", amounts.value[index - 1]);
});

const props = defineProps({
  amounts: {
    type: Array,
    default: () => [],
  },
});

const emit = defineEmits(["getAmountIndex"]);

const lineZero = computed(() => {
  return amountToPixels(0);
});

const amountToPixels = (amount) => {
  const min = Math.min(...amounts.value);
  const max = Math.max(...amounts.value);

  const minmax = Math.abs(max) + Math.abs(min); //Valor absoluto entre el inicio y el final
  const amountAbs = amount + Math.abs(min); //Calculo del valor absoluto de cada movimiento

  //Regla de tres minmax es el 100% el amountAbs es el X.
  //Esto se multiplica para 2 porque el grafico mide 200 px entonces yo quiero el 200%
  //Resto 200 al principio para que invierta el grafico y los negativos esten abajo y no arriba
  return GRAPHIC_HEIGHT - ((amountAbs * 100) / minmax) * 2;
};

const points = computed(() => {
  const total = amounts.value.length;
  return amounts.value.reduce((points, amount, i) => {
    //X (izquierda derecha) --> 300px longitud grafica / la cantidad de movimientos que tenemos y multiplicamos por cada iteracion
    const x = (GRAPHIC_WIDTH / total) * (i + 1);
    const y = amountToPixels(amount);
    return `${points} ${x},${y}`;
  }, `0, ${amountToPixels(amounts.value.length ? amounts.value[0] : 0)}`); //Inicializamos el grafico dependiendo si existe el primer movimiento
});

const tap = ({ target, touches }) => {
  showPointer.value = true;
  const elementWidth = target.getBoundingClientRect().width; //Width del grafico
  const elementX = target.getBoundingClientRect().x; //Distancia en X del grafico respecto a la pantalla
  const touchX = touches[0].clientX; //Posicion en X donde se ha echo el touch
  pointer.value = ((touchX - elementX) * GRAPHIC_WIDTH) / elementWidth;
};

const untap = () => {
  showPointer.value = false;
  emit("getAmountIndex", null);
};
</script>

<style scoped>
svg {
  width: 100%;
}

p {
  text-align: center;
}
</style>
