<script>
    import { Mail, Phone, MapPin, Eye, Pencil } from "lucide-svelte";
    export let name = "";
    export let email = "";
    export let phone = "";
    export let address = "";
    export let onView = () => {};
    export let onEdit = () => {};
</script>

<div
    class="bg-white border border-gray-200 rounded-xl shadow-sm p-5 w-full max-w-sm"
>
    <h2 class="text-lg font-semibold text-gray-900 mb-1">{name}</h2>
    <div class="flex items-center gap-2 text-gray-600 text-sm mb-1">
        <Mail size={16} />
        <span>{email}</span>
    </div>
    <div class="flex items-center gap-2 text-gray-600 text-sm mb-1">
        <Phone size={16} />
        <span>{phone}</span>
    </div>
    <div class="flex items-center gap-2 text-gray-600 text-sm mb-4">
        <MapPin size={16} />
        <span>{address}</span>
    </div>
    <div class="flex items-center gap-2 pt-2">
        <button
            on:click={onView}
            class="flex-1 flex cursor-pointer items-center justify-center gap-2 border border-gray-300 text-gray-700 text-sm px-3 py-2 rounded-lg hover:bg-gray-50 transition"
        >
            <Eye size={16} />
            <span>View</span>
        </button>

        <button
            on:click={onEdit}
            class="flex-1 flex cursor-pointer items-center justify-center gap-2 border border-gray-300 text-gray-700 text-sm px-3 py-2 rounded-lg hover:bg-gray-50 transition"
        >
            <Pencil size={16} />
            <span>Edit</span>
        </button>
    </div>
</div>
