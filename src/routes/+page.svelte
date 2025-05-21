<script>
  import { onMount } from 'svelte';
  let Graph = null;
  let clientReady = false;

  let rounds = [
    { estimated: 8, actual: 6 },
    { estimated: 10, actual: 9 },
    { estimated: 7, actual: 5 },
    { estimated: 9, actual: 10 }
  ];

  onMount(async () => {
  try {
    const module = await import('$lib/components/Graph.svelte');
    console.log('📦 Dynamisk modul importert:', module);
    
    if (!module?.default) {
      console.error('❌ Modul inneholder ikke en default eksport:', module);
      return;
    }

    Graph = module.default;
    clientReady = true;
    console.log('✅ Graph-komponent lastet inn og klar');
  } catch (err) {
    console.error('💥 Feil under dynamisk import:', err);
  }
});

</script>

<h1>🧪 Demo: Klient-side graf</h1>

{#if clientReady && Graph}
  <div class="border border-green-500 p-4">
    <svelte:component this={Graph} {rounds} />
  </div>
{:else}
  <p class="text-gray-500">⏳ Laster graf...</p>
{/if}
