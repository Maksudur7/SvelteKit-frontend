<script>
    import { onMount } from "svelte";
    import PersonCard from "$lib/components/PersonCard.svelte";
    import { goto } from "$app/navigation";

    let persons = []; 
    console.log(persons);
    let loading = true;
    onMount(async () => {
        try {
            const res = await fetch("https://sveltekit-backend-sigma.vercel.app/user");
            if (!res.ok) throw new Error("Failed to fetch data");
            const data = await res.json();
            persons = data; 
            console.log(persons);
        } catch (err) {
            console.error(err);
        } finally {
            loading = false;
        }
    });
    

    function handleView(person) {
        goto(`/persons/${person._id}`)
        
    }

    function handleEdit(person) {
        goto(`/persons/${person._id}/edit`);
    }
</script>

<div class="mx-auto px-6 py-10">
    <div class="flex items-center justify-between mb-10">
        <div>
            <h1 class="text-2xl font-semibold text-gray-900">Dashboard</h1>
            <p class="text-gray-500">Manage all your contacts in one place</p>
        </div>

        <a
            href="/persons/new"
            class="flex items-center gap-2 bg-blue-600 hover:bg-blue-700 text-white px-4 py-2 rounded-lg transition"
        >
            <span class="text-lg font-medium">＋</span>
            <span>Add New Person</span>
        </a>
    </div>

    {#if loading}
        <p class="text-center text-gray-500 mt-20">Loading persons...</p>

    {:else if persons.length > 0}
        <div
            class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4 justify-items-center"
        >
            {#each persons as person}
                <PersonCard
                    name={person.name}
                    email={person.email}
                    phone={person.phone}
                    address={person.address}
                    onView={() => handleView(person)}
                    onEdit={() => handleEdit(person)}
                   
                />
            {/each}
        </div>

    {:else}
        <div
            class="flex flex-col items-center justify-center text-center mt-16"
        >
            <div class="text-6xl mb-4 text-gray-800">👥</div>
            <h2 class="text-lg font-medium text-gray-700">No persons yet</h2>
            <p class="text-gray-500 mb-6">
                Get started by adding your first person to the system
            </p>
            <a
                href="/persons/new"
                class="flex items-center gap-2 bg-blue-600 hover:bg-blue-700 text-white px-5 py-2 rounded-lg transition"
            >
                <span class="text-lg cursor-pointer font-medium">＋</span>
                <span>Add Your First Person</span>
            </a>
        </div>
    {/if}
</div>
