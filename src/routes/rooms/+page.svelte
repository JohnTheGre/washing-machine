<script>
    let rooms = [];
    let error = null;
    let loading = true;
    import { onMount } from 'svelte';
    onMount(async () => {
        try {
            const response = await fetch('http://localhost:3000/api/rooms');
            const data = await response.json();
            rooms = data;
        } catch (err) {
            error = 'Error fetching data: ' + err.message;
        } finally {
            loading = false;
        }
    });
</script>

{#if loading}
    <p>Loading rooms...</p>
{:else if error}
    <p class="text-red-500">Error: {error}</p>
{:else}
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4 p-4">
        {#each rooms as room}
            <div class="p-4 rounded shadow bg-gradient-to-r from-green-100 to-lime-100 hover:from-green-200 hover:to-lime-200 transition-colors duration-300">
                <h2 class="text-xl font-bold mb-2">Room: {room.number}</h2>
                <p class="mb-2">Description: {room.name}</p>
                <h3 class="text-lg font-semibold mt-4 mb-2">Appliances:</h3>
                <ul class="list-disc list-inside">
                    {#each room.machines as appliance}
                        <li>{appliance}</li>
                    {/each}
                </ul>
            </div>
        {/each}
    </div>
{/if}