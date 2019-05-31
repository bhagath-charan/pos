<template>
  <div>
    <v-data-table :headers="headers" :items="pos.lineItems">
      <template v-slot:items="props">
        <td>{{ props.item.product.name }}</td>
        <td>{{ props.item.product.gst }}</td>
        <td>
          <v-edit-dialog :return-value.sync="props.item.quantity">
            {{props.item.quantity}}
            <template v-slot:input>
              <v-text-field v-model="props.item.quantity" label="Edit" single-line counter></v-text-field>
            </template>
          </v-edit-dialog>
        </td>
        <td>{{ props.item.product.price }}</td>
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
    pos: {
      customer: {},
      lineItems: [],
      totalAmount: 0
    },
    lineItem: {
      product: {},
      quantity: 0,
      discount: 0,
      amount: 0
    },
    editedIndex: -1,
  }),

  watch: {
    dialog(val) {
      val || this.close();
    }
  },

  created() {
    posEventBus.$on("selectedProduct", product => {
      this.lineItem.product = product;
      this.pos.lineItems.push(this.lineItem);
      this.lineItem = {
        product: {},
        quantity: 0,
        discount: 0,
        amount: 0
      };
    });
  },

  methods: {
    deleteItem(item) {
      const index = this.pos.lineItems.indexOf(item);
      confirm("Are you sure you want to delete this item?") &&
        this.pos.lineItems.splice(index, 1);
    },

    close() {
      this.dialog = false;
      setTimeout(() => {
        this.lineItem = Object.assign({}, this.lineItem);
        this.editedIndex = -1;
      }, 300);
    }
  }
};
</script>
