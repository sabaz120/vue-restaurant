<template>
  <div>
    <md-table v-model="contentData" table-header-color="green"
    >
      <md-table-row slot="md-table-row" slot-scope="{ item }">
        <md-table-cell md-label="Ingrediente" style="text-transform: capitalize;">{{ item.ingredient }}</md-table-cell>
        <md-table-cell md-label="Cantidad">{{ item.quantity }}</md-table-cell>
        <md-table-cell md-label="Fecha">{{ item.created_at }}</md-table-cell>
      </md-table-row>
    </md-table>
  </div>
</template>

<script>
export default {
  name: "ingredient-purchases-table",
  props: {
    content: {
      type: Array,
      default: () => [],
    },
  },
  data() {
    return {
      contentData: this.content.map(item => ({
        ingredient: item.ingredient,
        quantity: item.quantity,
        created_at: new Date(item.created_at).toISOString().slice(0, 19).replace('T', ' ')
      })),
    };
  },
  methods: {

  },
};
</script>