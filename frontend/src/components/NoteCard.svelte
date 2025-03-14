<script>
    export let note;
    export let onEdit;
    export let onDelete;
  </script>
  
  <div class="bg-white p-4 rounded-xl shadow-md max-w-md w-full">
    <div class="flex flex-col gap-2">
      <h2 class="text-lg font-bold">{note.judul}</h2>
      <p class="text-gray-600">{note.deskripsi}</p>
  
      <!-- Kategori -->
      <span class="inline-flex self-start bg-gray-300 text-gray-700 px-3 py-1 rounded-full text-sm">{note.kategori}</span>
    </div>
    <!-- Footer -->
    <div class="flex justify-between items-center mt-4 text-gray-500">
      <div class="flex flex-col gap-2">
        <span class="text-sm">🕒 {note.tanggal_dibuat || "Tanggal Tidak Tersedia"}</span>
        <span class="text-sm">🆕 {note.tanggal_diperbarui || "Tanggal Tidak Tersedia"}</span>
      </div>
      <div class="flex gap-2">
        <button on:click={() => onEdit(note)} class="text-blue-500 hover:text-blue-700 cursor-pointer">✏️</button>
        <button on:click={() => onDelete(note.id)} class="text-red-500 hover:text-red-700 cursor-pointer">🗑️</button>
      </div>
    </div>
  </div>
  