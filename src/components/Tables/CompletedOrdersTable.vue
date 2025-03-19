<template>
  <div>
    <md-table v-model="contentData" :table-header-color="tableHeaderColor">
      <md-table-row slot="md-table-row" slot-scope="{ item }">
        <md-table-cell md-label="#">{{ item.id }}</md-table-cell>
        <md-table-cell md-label="Receta">{{ item.recipe_name }}</md-table-cell>
        <md-table-cell md-label="Fecha">{{ item.created_at }}</md-table-cell>
      </md-table-row>
    </md-table>
  </div>
</template>

<script>
export default {
  name: "completed-orders-table",
  props: {
    tableHeaderColor: {
      type: String,
      default: "",
    },
    content: {
      type: Array,
      default: () => [],
    },
  },
  data() {
    return {
      contentData: this.content.map(item => ({
        id: item.id,
        recipe_name: item.recipe_name,
        created_at: new Date(item.created_at).toISOString().slice(0, 19).replace('T', ' ')
      })),
    };
  },
  mounted(){
  }
};
</script>
