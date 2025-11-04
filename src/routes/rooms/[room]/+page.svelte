<script>
    export let params;

    let room = null;
    let error = null;

    const loadRoomData = async () => {
        try {
            const response = await fetch(`http://localhost:3000/api/rooms/${params.room}`);
            if (!response.ok) {
                throw new Error('Room not found');
            }
            room = await response.json();
        } catch (err) {
            error = err.message;
        }
    };

    loadRoomData();
</script>

{#if error}
    <p class="text-red-500">Error: {error}</p>
{:else if !room}
    <p>Loading room data...</p>
{:else}
    <div class="p-4 bg-white rounded shadow">
        <h2 class="text-2xl font-bold mb-4">Room: {room.number}</h2>
        <p class="mb-2">Description: {room.name}</p>
        <h3 class="text-xl font-semibold mt-4 mb-2">Appliances: </h3>
        <ul class="list-disc list-inside">
            {#each room.machines as appliance}
                <li>{appliance}</li>
            {/each}
        </ul>
    </div>
{/if}
