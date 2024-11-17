<template>
  <div class="progress-container">
    <svg
      :width="size"
      :height="size"
      viewBox="0 0 100 100"
      class="progress-svg"
    >
      <!-- Базовый круг -->
      <circle
        cx="50"
        cy="50"
        r="45"
        fill="none"
        :stroke="baseColor"
        stroke-width="10"
      />
      <!-- Анимированный прогресс -->
      <circle
        cx="50"
        cy="50"
        r="45"
        fill="none"
        :stroke="progressColor"
        stroke-width="10"
        stroke-linecap="round"
        stroke-dasharray="282.6"
        :stroke-dashoffset="progressOffset"
        :style="{ transition: 'stroke-dashoffset 0.6s ease, stroke 0.6s' }"
        transform="rotate(-90 50 50)"
      />
    </svg>
    <!-- Текст внутри круга -->
    <div class="progress-text">
      <slot>{{ percentage }}%</slot>
    </div>
  </div>
</template>

<script>
  import { computed, defineComponent } from 'vue';

  export default defineComponent({
    props: {
      percentage: {
        type: Number,
        required: true,
        validator: value => value >= 0 && value <= 100,
      },
      size: {
        type: Number,
        default: 100,
      },
    },
    setup(props) {
      // Расчет смещения (анимация прогресса)
      const progressOffset = computed(() => {
        const totalCircumference = 2 * Math.PI * 45; // 2πr
        return totalCircumference * (1 - props.percentage / 100);
      });

      // Расчет цвета прогресса (градиент от красного к зеленому)
      const progressColor = computed(() => {
        const red = Math.max(255 - props.percentage * 2.55, 0);
        const green = Math.min(props.percentage * 2.55, 255);
        return `rgb(${red}, ${green}, 0)`;
      });

      // Цвет базового круга
      const baseColor = computed(() => '#e6e6e6');

      return { progressOffset, progressColor, baseColor };
    },
  });
</script>

<style scoped>
  /* Общий контейнер */
  .progress-container {
    display: inline-block;
    position: relative;
    width: 100%;
    height: 100%;
  }

  /* SVG прогресс */
  .progress-svg {
    transform: rotate(0deg);
  }

  /* Текст внутри прогресс-бара */
  .progress-text {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    font-size: 1.4em;
    font-weight: bold;
    font-family: Arial, sans-serif;
    color: #333;
  }

  /* Цвета для состояния */
  .progress-svg circle {
    transition: stroke-dashoffset 0.6s ease, stroke 0.6s ease;
  }
</style>
