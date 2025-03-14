<script>
  import { onMount } from "svelte";
  import axios from "axios";
  import NoteCard from "./components/NoteCard.svelte";

  let notes = [];
  let title = "";
  let description = "";
  let category = "";
  let editingNote = null;
  let showNotification = false;

  const API_URL = "http://localhost:5000/notes";

  // Mengambil semua catatan dari backend
  async function fetchNotes() {
    try {
      const response = await axios.get(API_URL);
      notes = response.data;
    } catch (error) {
      console.error("Error fetching notes:", error);
    }
  }

  onMount(fetchNotes);

  // Tambah / Perbarui Catatan
  async function saveNote() {
    if (!title.trim() || !description.trim() || !category.trim()) {
      alert("Semua field harus diisi!");
      return;
    }

    try {
      if (editingNote) {
        await axios.put(`${API_URL}/update-notes/${editingNote.id}`, {
          judul: title,
          deskripsi: description,
          kategori: category
        });
      } else {
        await axios.post(`${API_URL}/add-notes`, {
          judul: title,
          deskripsi: description,
          kategori: category
        });
      }

      title = "";
      description = "";
      category = "";
      editingNote = null;

      fetchNotes();
      showSuccessNotification(); // Tampilkan notifikasi
    } catch (error) {
      console.error("Error saving note:", error);
    }
  }

  // Notifikasi sukses
  function showSuccessNotification() {
    showNotification = true;
    setTimeout(() => {
      showNotification = false;
    }, 3000); // Hilang setelah 3 detik
  }

  // Edit catatan
  function editNote(note) {
    title = note.judul;
    description = note.deskripsi;
    category = note.kategori;
    editingNote = note;
  }

  // Hapus catatan
  async function deleteNote(id) {
    try {
      await axios.delete(`${API_URL}/delete-notes/${id}`);
      fetchNotes();
    } catch (error) {
      console.error("Error deleting note:", error);
    }
  }
</script>

<!-- Notifikasi -->
{#if showNotification}
  <div class="fixed top-4 right-4 bg-green-500 text-white px-4 py-2 rounded-md shadow-md">
    ✅ Catatan berhasil disimpan!
  </div>
{/if}

<main class="min-h-screen bg-gray-100 p-6">
  <div class="max-w-md mx-auto bg-white p-6 rounded-xl shadow-md">
    <h1 class="text-2xl font-bold text-center mb-4">📒 Notes App</h1>

    <!-- Form Input -->
    <div class="mb-4">
      <input bind:value={title} type="text" placeholder="Judul" class="w-full p-2 border rounded-md mb-2" />
      <textarea bind:value={description} placeholder="Deskripsi" class="w-full p-2 border rounded-md mb-2"></textarea>
      <input bind:value={category} type="text" placeholder="Kategori" class="w-full p-2 border rounded-md mb-2" />
      <button on:click={saveNote} class="w-full bg-blue-500 text-white p-2 rounded-md hover:bg-blue-700">
        {editingNote ? "Update Note" : "Add Note"}
      </button>
    </div>

    <!-- Daftar Catatan -->
    <div class="space-y-4">
      {#each notes as note}
        <NoteCard {note} onEdit={editNote} onDelete={deleteNote} />
      {/each}
    </div>
  </div>
</main>
