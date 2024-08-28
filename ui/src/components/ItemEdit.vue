<template>
  <div v-if="isVisible">
    <h2>Edit Mobil</h2>
    <form @submit.prevent="updateItem">
      <input v-model="name" placeholder="Nama Mobil" />
      <input v-model="warna" placeholder="Warna Mobil" />
      <input v-model="type" placeholder="Tipe Mobil" />
      <input v-model="tahun" placeholder="Tahun" />
      <input v-model="nopol" placeholder="Nopol" />
      <button type="submit">Update</button>
    </form>
  </div>
</template>

<script>
import axios from "axios";

export default {
  props: ["itemId"],
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
    updateItem() {
      axios
        .put(`http://localhost:8000/api/items/${this.itemId}`, {
          name: this.name,
          warna: this.warna,
          type: this.type,
          tahun: this.tahun,
          nopol: this.nopol,
        })
        .then(() => {
          this.$emit("item-updated");
          this.closeForm(); // Tutup form setelah item diupdate
        });
    },
    closeForm() {
      this.isVisible = false;
    },
  },
  mounted() {
    axios
      .get(`http://localhost:8000/api/items/${this.itemId}`)
      .then((response) => {
        const item = response.data;
        this.name = item.name;
        this.warna = item.warna;
        this.type = item.type;
        this.tahun = item.tahun;
        this.nopol = item.nopol;
      });
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
