<script>
  import { onMount } from 'svelte';
  import { tick } from 'svelte';

  let timeLeft = 60;
  let timer = null;
  let defaultTime = 60;
  let rounds = [];
  let newRound = { estimated: '', actual: '', improvement: '' };

  function formatTime(seconds) {
    const m = String(Math.floor(seconds / 60)).padStart(2, '0');
    const s = String(seconds % 60).padStart(2, '0');
    return `00:${m}:${s}`;
  }

  function adjustTime(amount) {
    timeLeft = Math.max(0, timeLeft + amount);
  }

  function startTimer() {
    if (timer) return;
    timer = setInterval(() => {
      if (timeLeft > 0) {
        timeLeft -= 1;
      } else {
        clearInterval(timer);
        timer = null;
        playSound();
      }
    }, 1000);
  }

  function playSound() {
    const audio = new Audio('/alarm.mp3');
    audio.play();
  }

  function addRound() {
    rounds = [...rounds, { ...newRound }];
    newRound = { estimated: '', actual: '', improvement: '' };
  }
</script>

<div class="p-4 max-w-3xl mx-auto space-y-6">
  <div class="flex items-center justify-center space-x-2 text-2xl font-bold text-indigo-700">
    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-8 h-8">
      <path stroke-linecap="round" stroke-linejoin="round" d="M12 3v2.25M12 18.75V21m6.364-13.614l-1.591 1.591M6.227 17.773l-1.591 1.591m13.728 0l-1.591-1.591M6.227 6.227L4.636 4.636M21 12h-2.25M5.25 12H3" />
    </svg>
    <span>Magic Ball Productions</span>
  </div>

  <div class="flex items-center space-x-4 justify-center">
    <button on:click={() => adjustTime(-30)} class="px-4 py-2 rounded bg-gray-300">
      <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6">
        <path stroke-linecap="round" stroke-linejoin="round" d="M19.5 12h-15" />
      </svg>
    </button>
    <div class="text-4xl font-mono">{formatTime(timeLeft)}</div>
    <button on:click={() => adjustTime(30)} class="px-4 py-2 rounded bg-gray-300">
      <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6">
        <path stroke-linecap="round" stroke-linejoin="round" d="M12 4.5v15m7.5-7.5h-15" />
      </svg>
    </button>
  </div>

  <div class="text-center">
    <button on:click={startTimer} class="px-6 py-2 rounded bg-blue-500 text-white">Start</button>
  </div>

  <div class="rounded-xl border border-gray-300 p-4">
    <div class="grid grid-cols-5 font-semibold border-b pb-2 mb-2">
      <div>Runde</div>
      <div>Estimert</div>
      <div>Faktisk</div>
      <div class="col-span-2">Tiltak</div>
    </div>

    {#each rounds as round, i}
      <div class="grid grid-cols-5 mb-2">
        <div>{i + 1}</div>
        <div>{round.estimated}</div>
        <div>{round.actual}</div>
        <div class="col-span-2">{round.improvement}</div>
      </div>
    {/each}

    <div class="grid grid-cols-5 gap-2 mt-4 items-center">
      <div class="text-center text-gray-500">{rounds.length + 1}</div>
      <input bind:value={newRound.estimated} placeholder="Estimert" class="border p-2 rounded" />
      <input bind:value={newRound.actual} placeholder="Faktisk" class="border p-2 rounded" />
      <input bind:value={newRound.improvement} placeholder="Tiltak" class="border p-2 rounded col-span-2" />
    </div>
    <div class="mt-2">
      <button on:click={addRound} class="px-4 py-2 bg-green-500 text-white rounded">Legg til runde</button>
    </div>
  </div>
</div>

<style>
  input {
    width: 100%;
  }
</style>