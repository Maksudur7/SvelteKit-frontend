<script>
    import { onMount } from "svelte";
    import { page } from "$app/stores";
    import { goto } from "$app/navigation";

    let person = null;
    let id;
    let name = "";
    let email = "";
    let phone = "";
    let address = "";

    $: id = $page.params.id;

    onMount(async () => {
        try {
            const res = await fetch(`http://localhost:5000/user`);
            if (!res.ok) throw new Error("Failed to fetch person");
            const data = await res.json();
            person = data.find((user) => user._id === id);
            name = person.name;
            email = person.email;
            phone = person.phone;
            address = person.address;

            console.log("Fetched person:", person);
        } catch (err) {
            console.error("Error fetching person:", err);
        }
    });

    const handleUpdate = async (event) => {
        event.preventDefault();

        const updatedData = { name, email, phone, address };

        try {
            const res = await fetch(`http://localhost:5000/user/${id}`, {
                method: "PATCH",
                headers: { "Content-Type": "application/json" },
                body: JSON.stringify(updatedData),
            });

            if (!res.ok) throw new Error("Failed to update person");

            const result = await res.json();
            console.log("Updated person:", result);
            goto("/");
        } catch (err) {
            console.error("Error while updating person:", err);
        }
    };
</script>

<div class="max-w-lg mx-auto px-6 py-10">
    <a
        href="/"
        class="flex items-center text-gray-600 hover:text-gray-800 mb-6 gap-1 text-sm font-medium"
    >
        <span>←</span>
        <span>Back to Dashboard</span>
    </a>

    <div class="bg-white shadow-md rounded-2xl p-8 border border-gray-100">
        <h1 class="text-2xl font-semibold text-gray-900 mb-1">Edit Person</h1>
        <p class="text-gray-500 mb-8">
            Update the details below and save changes
        </p>

        <form on:submit={handleUpdate} class="space-y-5">
            <div>
                <label class="block text-gray-700 font-medium mb-1"
                    >Name <span class="text-red-500">*</span></label
                >
                <input
                    type="text"
                    bind:value={name}
                    required
                    class="w-full border border-gray-300 rounded-lg px-3 py-2 focus:outline-none focus:ring-2 focus:ring-blue-500"
                />
            </div>
            <div>
                <label class="block text-gray-700 font-medium mb-1"
                    >Email <span class="text-red-500">*</span></label
                >
                <input
                    type="email"
                    bind:value={email}
                    required
                    class="w-full border border-gray-300 rounded-lg px-3 py-2 focus:outline-none focus:ring-2 focus:ring-blue-500"
                />
            </div>
            <div>
                <label class="block text-gray-700 font-medium mb-1">Phone</label
                >
                <input
                    type="tel"
                    bind:value={phone}
                    class="w-full border border-gray-300 rounded-lg px-3 py-2 focus:outline-none focus:ring-2 focus:ring-blue-500"
                />
            </div>
            <div>
                <label class="block text-gray-700 font-medium mb-1"
                    >Address</label
                >
                <textarea
                    rows="2"
                    bind:value={address}
                    class="w-full border border-gray-300 rounded-lg px-3 py-2 focus:outline-none focus:ring-2 focus:ring-blue-500"
                ></textarea>
            </div>
            <div class="flex items-center justify-end gap-3 pt-4">
                <button
                    type="button"
                    on:click={() => goto("/")}
                    class="px-5 py-2 border border-gray-300 rounded-lg hover:bg-gray-50 transition"
                    >Cancel</button
                >
                <button
                    type="submit"
                    class="px-6 py-2 bg-blue-600 cursor-pointer text-white rounded-lg hover:bg-blue-700 transition"
                    >Update Person</button
                >
            </div>
        </form>
    </div>
</div>
