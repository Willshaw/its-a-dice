<script setup lang="ts">
useSeoMeta({
  title: 'Many Dice',
  ogTitle: 'May Dice',
  description: 'For use when you don\'t have a dice, but do have a device (and an internet connection and at least 1 finger)',
  ogDescription: 'For use when you don\'t have a dice, but do have a device (and an internet connection and at least 1 finger)',
})

// number of dice and number of sides on each die
const num_rolls = defineModel('num_rolls', { type: Number, default: 1 });
const num_sides = defineModel('num_sides', { type: Number, default: 6 });

// history of rolls, which could have been more than 1 die
const history = ref([]);

// function to return a random number between 1 and the limit, defaulting to 6
const single_roll = ((limit = 6) => { return Math.floor(Math.random() * limit) + 1; });

// make a certain number of rolls (each one to the limit), returning an array of the results
const make_rolls = () => {
  const rolls: number[] = [];
  for (let i = 0; i < num_rolls.value; i++) {
    rolls.push(single_roll(num_sides.value));
  }
  // push the rolls into start of the history
  history.value.unshift(rolls.sort());
  console.log(rolls);
};
</script>

<template>
  <div class="w-full max-w-screen-xl m-auto">
    <h1>Dice Dice Dice!</h1>
    <div class="controls flex">
      <div class="control">
        <h3>How many dice?</h3>
        <input placeholder="6" type="number" v-model="num_rolls" />
      </div>
      <div class="control">
        <h3>How many sides?</h3>
        <input placeholder="6" type="number" v-model="num_sides" />
      </div>
      <div class="control align-baseline flex-col content-end">
        <button @click="make_rolls">Roll</button>
      </div>
    </div>
    <h2 class="my-4">History</h2>
    <div class="history">
      <div class="roll flex" v-for="rolls in history" :key="rolls">
        <div class="face" :class="{ critical: roll === 6, miss: roll === 1 }" v-for="roll in rolls" :key="roll">{{ roll }}</div>
      </div>
    </div>
  </div>
</template>

<style>
  h1 {
    font-size: 2rem;
    margin-bottom: 1rem;
  }

  h2 {
    font-size: 1.5rem;
    margin-bottom: 1rem;
  }

  input {
    @apply p-2 border border-gray-300 rounded;
  }

  button {
    @apply bg-blue-500 text-white p-2 rounded;
  }

  .control {
    @apply mr-4;
  }

  .roll {
    @apply bg-gray-100 my-2 p-4;
  }

  .face {
    @apply bg-gray-700 text-white font-bold mr-2 p-2 rounded w-12 h-12 flex items-center justify-center;

    &.critical {
      @apply bg-red-600;
    }

    &.miss {
      @apply bg-gray-300 text-black;
    }
  }
</style>
