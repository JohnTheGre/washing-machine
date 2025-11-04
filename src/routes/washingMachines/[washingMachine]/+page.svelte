<script>
    export let params;
    let washingMachine = null;
    let error = null;
    let newMinutes = 0;

    const loadWashingMachineData = async () => {
        try {
            const response = await fetch(`http://localhost:3000/api/washing-machines/${params.washingMachine}`);
            if (!response.ok) {
                throw new Error('Washing machine not found');
            }
            washingMachine = await response.json();
        } catch (err) {
            error = err.message;
        }
    };

    loadWashingMachineData();

    async function setTimer() {
        const response = await fetch(`http://localhost:3000/api/washing-machines/${params.washingMachine}/timer`, {
            method: "PUT",
            headers: { "Content-Type": "application/json" },
            body: JSON.stringify({ timer: parseInt(newMinutes) })
        });

        if (response.ok) {
            loadWashingMachineData(); // refresh data
        } else {
            alert("Error setting timer");
        }
    }

    async function deleteWashingMachine() {
        const response = await fetch(`http://localhost:3000/api/washing-machines/${params.washingMachine}`, {
            method: "DELETE"
        });

        if (response.ok) {
            // Redirect to washing machines list after deletion
            window.location.href = '/washingMachines';
        } else {
            alert("Error deleting washing machine");
        }
    }
</script>

{#if error}
    <p class="text-red-500">Error: {error}</p>
{:else if !washingMachine}
    <p>Loading washing machine data...</p>
{:else}
    <div class="p-4 border-blue-500 rounded-lg p-4 bg-gradient-to-r from-sky-100 to-blue-100 hover:from-sky-200 hover:to-blue-200 transition-colors duration-300">
        <h2 class="text-2xl font-bold mb-4">Washing Machine: {washingMachine.machineNumber}</h2>
        <p class="mb-2">Location: {washingMachine.location}</p>
        <p class="mb-2">Status: {washingMachine.status}</p>
        <p class="text-xl font-semibold mt-4 mb-2">Timer:</p>
        {#if washingMachine.remaining > 0}
         <p>{washingMachine.remaining} minutes remaining</p>
        {:else}
            <p>No active timer</p>
            <input type="number" bind:value={newMinutes} min="1" class="border p-2 mr-2" placeholder="Minutes" />
            <button on:click={setTimer} class="bg-blue-500 text-white p-2 rounded cursor-pointer">
                Set Timer
            </button>
            <button on:click={deleteWashingMachine} class="bg-red-500 text-white p-2 rounded cursor-pointer">
                Delete Machine
            </button>
        {/if}
 
    </div>
{/if}
