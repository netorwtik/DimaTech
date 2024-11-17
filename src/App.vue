<template>
  <div class="app">
    <div class="progress-examples">
      <CircleProgress :percentage="percentage" :size="150" />
      <div class="controls">
        <label>
          Процент:
          <input type="range" min="0" max="100" v-model="percentage" />
        </label>
      </div>
    </div>
  </div>
</template>

<script>
  import CircleProgress from './components/CircleProgress.vue';

  export default {
    components: { CircleProgress },
    data() {
      return {
        percentage: 50, // Текущее значение прогресса
      };
    },
  };
</script>

<style>
  .app {
    text-align: center;
    margin: 20px;
  }

  .progress-examples {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .controls {
    margin-top: 20px;
  }

  input[type='range'] {
    width: 300px;
  }
</style>
