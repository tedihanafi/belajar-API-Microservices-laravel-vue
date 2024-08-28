<template>
  <div>
    <h1>DATA MOBIL KAMI</h1>
    <button @click="showCreateForm" class="btn btn-add">+ Mobil</button>
    <ItemCreate v-if="showCreate" @item-added="fetchItems" />
    <ItemEdit
      v-if="showEdit"
      :item-id="currentItemId"
      @item-updated="fetchItems"
    />

    <hr />
    <table class="item-table">
      <thead>
        <tr>
          <th>Nama Mobil</th>
          <th>Warna</th>
          <th>Type</th>
          <th>Tahun</th>
          <th>No-Pol</th>
          <th>Options</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in items" :key="item.id">
          <td>{{ item.name }}</td>
          <td>{{ item.warna }}</td>
          <td>{{ item.type }}</td>
          <td>{{ item.tahun }}</td>
          <td>{{ item.nopol }}</td>
          <td>
            <button @click="editItem(item.id)" class="btn btn-edit">
              Edit
            </button>
            <button @click="deleteItem(item.id)" class="btn btn-delete">
              Delete
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from "axios";
import ItemCreate from "./ItemCreate.vue";
import ItemEdit from "./ItemEdit.vue";

export default {
  components: { ItemCreate, ItemEdit },
  data() {
    return {
      items: [],
      showCreate: false,
      showEdit: false,
      currentItemId: null,
    };
  },
  methods: {
    fetchItems() {
      axios.get("http://localhost:8000/api/items").then((response) => {
        this.items = response.data;
      });
    },
    deleteItem(id) {
      axios.delete(`http://localhost:8000/api/items/${id}`).then(() => {
        this.fetchItems();
      });
    },
    editItem(id) {
      this.currentItemId = id;
      this.showEdit = true;
    },
    showCreateForm() {
      this.showCreate = true;
    },
  },
  mounted() {
    this.fetchItems();
  },
};
</script>

<style>
/* itemslist component styles */
body {
  width: 100%;
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 0;
}

div {
  width: 80%;
  margin: 10px auto;
  padding-top: 20px;
  border-radius: 8px;
}

h1 {
  font-size: 1.5em;
  color: #333;
  margin-bottom: 20px;
}

button {
  padding: 8px 12px;
  border: none;
  border-radius: 4px;
  color: #ffffff;
  cursor: pointer;
  font-size: 0.875em;
  transition: background-color 0.2s ease;
}

.btn-edit {
  background-color: #007bff;
}

.btn-edit:hover {
  background-color: #0056b3;
}

.btn-delete {
  background-color: #dc3545;
}

.btn-delete:hover {
  background-color: #c82333;
}

.btn-add {
  background-color: #28a745;
  margin-bottom: 20px;
}

.btn-add:hover {
  background-color: #218838;
}

hr {
  border: none;
  border-top: 1px solid #e0e0e0;
  margin: 20px 0;
}

.item-table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 10px;
}

.item-table th,
.item-table td {
  padding: 12px;
  text-align: left;
  border-bottom: 1px solid #ddd;
}

.item-table th {
  background-color: #f4f4f4;
  font-weight: bold;
  text-align: center;
}

.item-table tr:hover {
  background-color: #f1f1f1;
}

.item-table td {
  text-align: center;
}

.item-table .btn-edit,
.item-table .btn-delete {
  margin: 0 5px;
}
</style>
