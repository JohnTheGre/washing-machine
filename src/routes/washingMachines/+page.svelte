<script>
    let washingMachines = [];
    let error = null;
    let loading = true;
    // let newMinutes = 0;

    import { onMount } from 'svelte';
    onMount(() => {
        async function fetchData() {
            try {
                const response = await fetch('http://localhost:3000/api/washing-machines');
                const data = await response.json();
                washingMachines = data;
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

    // async function setTimer() {
    //     try {
    //         const response = await fetch(`http://localhost:3000/api/washing-machines/${washingMachines[0].id}/set-timer`, {
    //             method: 'POST',
    //             headers: {
    //                 'Content-Type': 'application/json'
    //             },
    //             body: JSON.stringify({ minutes: newMinutes })
    //         });
    //         if (!response.ok) {
    //             throw new Error('Failed to set timer');
    //         }
    //         // Refresh data after setting timer
    //         const data = await response.json();
    //         washingMachines = washingMachines.map(machine =>
    //             machine.id === data.id ? data : machine
    //         );
    //     } catch (err) {
    //         error = 'Error setting timer: ' + err.message;
    //     }
    // }   
</script>

{#if loading}
    <p>Loading washing machines...</p>
{:else if error}
    <p class="text-red-500">Error: {error}</p>
{:else}
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4 p-4 ">
        {#each washingMachines as machine}
            <div class=" p-4 rounded shadow bg-gradient-to-r from-sky-100 to-blue-100 hover:from-sky-200 hover:to-blue-200 transition-colors duration-300">
                <a href={`/washingMachines/${machine.id}`}>
                    <h2 class="text-xl font-bold mb-2">Washing Machine: {machine.machineNumber}</h2>
                    <p class="mb-2">Location: {machine.location}</p>
                    <p class="mb-2">Status: {machine.status}</p>
                    <p class="mb-2">Time Left: {machine.remaining} min</p>
                </a>
            </div>
        {/each}
    </div>
{/if}