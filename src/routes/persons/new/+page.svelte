<script>
    import { goto } from "$app/navigation";
    import toastr from 'toastr';
    import 'toastr/build/toastr.min.css';

    let name = "";
    let email = "";
    let phone = "";
    let address = "";

    async function handleSubmit(event) {
        event.preventDefault();

        const newPerson = { name, email, phone, address };

        try {
            const res = await fetch("https://sveltekit-backend-sigma.vercel.app/user", {
                method: "POST",
                headers: { "Content-Type": "application/json" },
                body: JSON.stringify(newPerson),
            });

            if (res.ok) {
                toastr.success("Person added successfully!");
                goto("/");
            } else {
                toastr.error("Failed to add person");
            }
        } catch (err) {
            console.error("Error while submitting:", err);
            toastr.error("Server unreachable!");
        }
    }
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
        <h1 class="text-2xl font-semibold text-gray-900 mb-1">
            Add New Person
        </h1>
        <p class="text-gray-500 mb-8">
            Fill in the details to add a new person
        </p>

        <form on:submit={handleSubmit} class="space-y-5">
            <div>
                <label class="block text-gray-700 font-medium mb-1"
                    >Name <span class="text-red-500">*</span></label
                >
                <input
                    type="text"
                    placeholder="John Smith"
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
                    placeholder="example@gmail.com"
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
                    placeholder="+1 (555) 123-4567"
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
                    placeholder="123 Main St, City, State, ZIP"
                    bind:value={address}
                    class="w-full border border-gray-300 rounded-lg px-3 py-2 focus:outline-none focus:ring-2 focus:ring-blue-500"
                ></textarea>
            </div>
            <div class="flex items-center justify-end gap-3 pt-4">
                <button
                    type="button"
                    on:click={() => goto("/")}
                    class="px-5 py-2 cursor-pointer border border-gray-300 rounded-lg hover:bg-gray-50 transition"
                >
                    Cancel
                </button>

                <button
                    type="submit"
                    class="px-6 py-2 cursor-pointer bg-blue-600 text-white rounded-lg hover:bg-blue-700 transition"
                >
                    Create Person
                </button>
            </div>
        </form>
    </div>
</div>
