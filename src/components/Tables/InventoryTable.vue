<template>
  <div>
    <md-table v-model="contentData" table-header-color="green"
    >
      <md-table-row slot="md-table-row" slot-scope="{ item }">
        <md-table-cell md-label="Nombre" style="text-transform: capitalize;">{{ item.ingredient }}</md-table-cell>
        <md-table-cell md-label="Cantidad disponible">Disponible: {{ item.quantity }}</md-table-cell>
      </md-table-row>
    </md-table>
  </div>
</template>

<script>
export default {
  name: "inventory-table",
  props: {
    content: {
      type: Array,
      default: () => [],
    },
  },
  data() {
    return {
      contentData: this.content,
    };
  },
  methods: {

  },
};
</script>