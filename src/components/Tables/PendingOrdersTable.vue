<template>
  <div>
    <md-table v-model="contentData" :table-header-color="tableHeaderColor">
      <md-table-row slot="md-table-row" slot-scope="{ item }">
        <md-table-cell md-label="#">{{ item.id }}</md-table-cell>
        <md-table-cell md-label="Receta">{{ item.recipe_name }}</md-table-cell>
        <md-table-cell md-label="Estado">{{ item.status }}</md-table-cell>
        <md-table-cell md-label="Fecha">{{ item.created_at }}</md-table-cell>
      </md-table-row>
    </md-table>
  </div>
</template>

<script>
export default {
  name: "pending-orders-table",
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
  methods:{
    parseStatusName(statusName){
      switch (statusName) {
        case 'pending':
          return 'Pendiente';
          break;
        case 'preparing':
          return 'Preparando';
          break;
        case 'completed':
          return 'Completado';
          break;
        default:
          return 'Pendiente';
          break;
      }
    }
  },
  data() {
    return {
      contentData: this.content.map(item => ({
        id: item.id,
        recipe_name: item.recipe_name,
        status: this.parseStatusName(item.status),
        created_at: new Date(item.created_at).toISOString().slice(0, 19).replace('T', ' ')
      })),
    };
  },
  mounted(){
  }
};
</script>
