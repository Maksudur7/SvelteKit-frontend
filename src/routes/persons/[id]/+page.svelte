<script>
    import { onMount } from "svelte";
    import { page } from "$app/stores";
    import swal from "sweetalert";
    import toastr from "toastr";
    import { goto } from "$app/navigation";

    let person = null;
    let id;
    $: id = $page.params.id;
    onMount(async () => {
        console.log("Received ID from URL:", id);

        try {
            const res = await fetch(`https://sveltekit-backend-sigma.vercel.app/user`);
            if (!res.ok) throw new Error("Failed to fetch person");

            const data = await res.json();
            person = data.find((user) => user._id === id);
            console.log("Fetched person:", person);
        } catch (err) {
            console.error("Error while fetching person:", err);
        }
    });

    async function handelDelet(id) {
        console.log(id);

        try {
            const res = await fetch(`https://sveltekit-backend-sigma.vercel.app/user/${id}`, {
                method: "DELETE",
            });
            if (res.ok) {
                toastr.error("Delet Successfuly");
                goto("/");
            }
        } catch (err) {
            console.error("Error deleting user:", err);
        }
    }

    async function handelEdit() {
        goto(`/persons/${person._id}/edit`);
    }
</script>

{#if person}
    <div class=" bg-white rounded-lg shadow p-6 space-y-4 m-20">
        <div class="flex items-center justify-between">
            <div>
                <h1 class="text-2xl font-semibold text-gray-800">
                    {person.name}
                </h1>
                <p class="text-sm text-gray-500">
                    Added on {person.date}
                </p>
            </div>
            <span
                class="text-xs bg-green-100 text-green-800 px-2 py-1 rounded-full"
            >
                {person.status ? person.status : "Active"}
            </span>
        </div>

        <div class="border-t pt-4 space-y-3">
            <p>📧 {person.email}</p>
            <p>📞 {person.phone}</p>
            <p>📍 {person.address}</p>
        </div>
        <div class="flex justify-end gap-3 border-t pt-4">
            <button
            onclick={() =>handelEdit()}
                class="flex cursor-pointer items-center gap-1 text-gray-700 bg-gray-100 hover:bg-gray-200 px-4 py-2 rounded-md transition"
            >
                ✏️ Edit
            </button>
            <button
                onclick={() => handelDelet(person._id)}
                class="flex cursor-pointer items-center gap-1 text-white bg-red-600 hover:bg-red-700 px-4 py-2 rounded-md transition"
            >
                🗑️ Delete
            </button>
        </div>
    </div>
{:else}
    <p class="text-center mt-10">Loading person details...</p>
{/if}
