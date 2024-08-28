<template>
  <div v-if="isVisible">
    <h2>Tambahkan Mobil Baru</h2>
    <form @submit.prevent="createItem">
      <input v-model="name" placeholder="Nama Mobil" />
      <input v-model="warna" placeholder="Warna Mobil" />
      <input v-model="type" placeholder="Tipe Mobil" />
      <input v-model="tahun" placeholder="Tahun" />
      <input v-model="nopol" placeholder="Nopol" />
      <button type="submit">Create</button>
    </form>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      name: "",
      warna: "",
      type: "",
      tahun: "",
      nopol: "",
      isVisible: true, // Kontrol visibility form
    };
  },
  methods: {
    createItem() {
      axios
        .post("http://localhost:8000/api/items", {
          name: this.name,
          warna: this.warna,
          type: this.type,
          tahun: this.tahun,
          nopol: this.nopol,
        })
        .then(() => {
          this.$emit("item-added");
          this.resetForm();
          this.closeForm(); // Tutup form setelah item ditambahkan
        });
    },
    resetForm() {
      this.name = "";
      this.warna = "";
      this.type = "";
      this.tahun = "";
      this.nopol = "";
    },
    closeForm() {
      this.isVisible = false;
    },
  },
};
</script>

<style>
h2 {
  color: #333;
  font-size: 1.5em;
  margin-bottom: 20px;
}

form {
  display: flex;
  flex-direction: column;
}

input {
  padding: 10px;
  margin-bottom: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 1em;
}

button {
  padding: 10px 15px;
  border: none;
  border-radius: 4px;
  color: #fff;
  background-color: #007bff;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: #0056b3;
}
</style>
