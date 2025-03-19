<template>
  <div>
    <md-table v-model="contentData" table-header-color="green"
    >
      <md-table-row slot="md-table-row" slot-scope="{ item }">
        <md-table-cell md-label="Nombre">{{ item.name }}</md-table-cell>
        <md-table-cell md-label="Ingredientes">{{ item.ingredients }}.</md-table-cell>
      </md-table-row>
    </md-table>
  </div>
</template>

<script>
export default {
  name: "recipes-table",
  props: {
    content: {
      type: Array,
      default: () => [],
    },
  },
  data() {
    return {
      contentData: this.content.map(item => ({
        name: item.name,
        ingredients: item.ingredients.map(ingredient => `${ingredient.ingredient}: ${ingredient.quantity}`).join(', ')
      })),
    };
  },
  methods: {

  },
};
</script>