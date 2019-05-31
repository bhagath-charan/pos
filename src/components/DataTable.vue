<template>
  <div>
    <v-data-table :headers="headers" :items="pos.products">
      <template v-slot:items="props">
        <td>{{ props.item.name }}</td>
        <td >{{ props.item.gst }}</td>
        <td>
          <v-edit-dialog
            :return-value.sync="pos.quantity"
            lazy
            @save="save"
            @cancel="cancel"
            @open="open"
            @close="close"
          >
            {{pos.quantity}}
            <template v-slot:input>
              <v-text-field v-model="pos.quantity" label="Edit" single-line counter></v-text-field>
            </template>
          </v-edit-dialog>
        </td>
        <td>{{ props.item.price }}</td>
        <td>
          <v-icon small @click="deleteItem(props.item)">delete</v-icon>
        </td>
      </template>
    </v-data-table>
  </div>
</template>

<script>
import { posEventBus } from "../main";
export default {
  name: "DataTable",
  data: () => ({
    dialog: false,
    headers: [
      {
        text: "Product",
        align: "left",
        sortable: false,
        value: "name"
      },
      { text: "GST", value: "gst" },
      { text: "Quantity", value: "quantity" },
      { text: "Price", value: "price" },
      { text: "Actions", value: "name", sortable: false }
    ],
    pos : {
      customer : {},
      products : [],
      quantity : 0,
      totalAmount : 0
    },
    editedIndex: -1,
    editedItem: {
      name: "",
      gst: 0,
      price: 0
    },
    defaultItem: {
      name: "",
      gst: 0,
      price: 0
    }
  }),

  computed: {
    formTitle() {
      return this.editedIndex === -1 ? "New Item" : "Edit Item";
    }
  },

  watch: {
    dialog(val) {
      val || this.close();
    }
  },

  created() {
    this.initialize();
    posEventBus.$on("selectedProduct", product => {
      this.pos.products.push(product);
    });
  },

  methods: {
    initialize() {
      this.pos.products = [];
    },
    editItem(item) {
      this.editedIndex = this.pos.products.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.dialog = true;
    },

    deleteItem(item) {
      const index = this.pos.products.indexOf(item);
      confirm("Are you sure you want to delete this item?") &&
        this.pos.products.splice(index, 1);
    },

    close() {
      this.dialog = false;
      setTimeout(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
        this.editedIndex = -1;
      }, 300);
    },

    save() {
      if (this.editedIndex > -1) {
        Object.assign(this.pos.products[this.editedIndex], this.editedItem);
      } else {
        this.pos.products.push(this.editedItem);
      }
      this.close();
    }
  }
};
</script>
