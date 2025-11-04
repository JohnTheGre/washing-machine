<script>
    let dryers = [];
    let error = null;
    let loading = true;
    import { onMount } from 'svelte';
    onMount(() => {
        async function fetchData() {
            try {
                const response = await fetch('http://localhost:3000/api/dryers');
                const data = await response.json();
                dryers = data;
        } catch (err) {
            error = 'Error fetching data: ' + err.message;
        } finally {
            loading = false;
        }
    }

    fetchData();

    // refresh every 30 seconds
    const interval = setInterval(fetchData, 30000);

    return () => clearInterval(interval);
});
</script>

{#if loading}
    <p>Loading dryers...</p>
{:else if error}
    <p class="text-red-500">Error: {error}</p>
{:else}
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4 p-4">
        {#each dryers as dryer}
            <div class="p-4 rounded shadow  border-yellow-500 rounded-lg p-4 bg-gradient-to-r from-yellow-100 to-yellow-200 hover:from-yellow-200 hover:to-yellow-300 transition-colors duration-300">
                <a href={`/dryers/${dryer.id}`}>
                    <h2 class="text-xl font-bold mb-2">Dryer: {dryer.dryerNumber}</h2>
                    <p class="mb-2">Location: {dryer.location}</p>
                    <p class="mb-2">Status: {dryer.status}</p>
                    <p class="mb-2">Timer: {dryer.remaining} min</p>
                </a>
            </div>
        {/each}
    </div>
{/if}
