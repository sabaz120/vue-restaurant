<template>
  <div class="content">
    <div class="md-layout">
      <div class="md-layout-item md-size-100">
        <div class="pull-left">
          <h2 class="title">
            Jornada de almuerzos gratis en Restaurant Mr. Tomato!
          </h2>
        </div>
        <div class="pull-right">
          <md-button :disabled="loadingRequestOrder" class="md-raised md-success" @click="requestOrder()">
            <span v-if="!loadingRequestOrder">Solicitar un plato</span>
            <span v-else-if="loadingRequestOrder">Procesando...</span>
          </md-button>
        </div>
      </div>
      <div class="md-layout-item md-medium-size-100 md-xsmall-size-100 md-size-50">
        <md-card>
          <md-card-header data-background-color="blue">
            <h4 class="title">Pedidos en proceso</h4>
            <p class="category">Últimos pedidos solicitados</p>
          </md-card-header>
          <md-card-content>
            <pending-orders-table v-if="!loadingPendingOrders" :content="pendingOrders" table-header-color="blue">
            </pending-orders-table>
            <ThePagination v-if="!loadingPendingOrders && pendingOrders.length>0" v-model="pendingdOrdersPagination.current_page" :totalPages="pendingdOrdersPagination.last_page"
            @change="getPendingOrders()" />
            <spinner v-if="loadingPendingOrders"></spinner>
          </md-card-content>
        </md-card>
      </div>
      <div class="md-layout-item md-medium-size-100 md-xsmall-size-100 md-size-50">
        <md-card>
          <md-card-header data-background-color="green">
            <h4 class="title">Pedidos procesados</h4>
            <p class="category">Últimos pedidos procesados</p>
          </md-card-header>
          <md-card-content>
            <completed-orders-table v-if="!loadingCompletedOrders" :content="completedOrders"
              table-header-color="green">
            </completed-orders-table>
            <ThePagination v-if="!loadingCompletedOrders && completedOrders.length>0" v-model="completedOrdersPagination.current_page" :totalPages="completedOrdersPagination.last_page"
              @change="getCompletedOrders()" />
            <spinner v-if="loadingCompletedOrders"></spinner>
          </md-card-content>
        </md-card>
      </div>
      <div class="md-layout-item md-medium-size-100 md-xsmall-size-100 md-size-50">
        <md-card>
          <md-card-header data-background-color="blue">
            <h4 class="title">Compras de ingredientes en plaza</h4>
            <p class="category">Últimas compras realizadas, esta lista se actualizará cada 20 segundos</p>
          </md-card-header>
          <md-card-content>
            <ingredient-purchases-table v-if="!loadingPurchases" :content="ingredientPurchases">
            </ingredient-purchases-table>
            <ThePagination v-if="!loadingPurchases && ingredientPurchases.length>0" v-model="purchasesPagination.current_page" :totalPages="purchasesPagination.last_page"
            @change="getPurchases()" />
            <spinner v-if="loadingPurchases"></spinner>
          </md-card-content>
        </md-card>
      </div>
      <div class="md-layout-item md-medium-size-100 md-xsmall-size-100 md-size-50">
        <nav-tabs-card>
          <template slot="content">
            <span class="md-nav-tabs-title">
              Datos:
            </span>
            <md-tabs class="md-success" md-alignment="left">
              <md-tab id="tab-home" md-label="Inventario" md-icon="edit">
                <inventory-table v-if="!loadingInventory" :content="inventoryIngredients">
                </inventory-table>
                <ThePagination v-if="!loadingInventory && inventoryIngredients.length>0" v-model="inventoryPagination.current_page" :totalPages="inventoryPagination.last_page"
                @change="getInventory()" />
                <spinner v-if="loadingInventory"></spinner>
              </md-tab>

              <md-tab id="tab-posts" md-label="Recetas" md-icon="book">
                <recipes-table v-if="!loadingRecipes" :content="recipesData"></recipes-table>
                <ThePagination v-if="!loadingRecipes" v-model="recipePagination.current_page" :totalPages="recipePagination.last_page"
                @change="getRecipes()" />
                <spinner v-if="loadingRecipes"></spinner>
              </md-tab>
            </md-tabs>
          </template>
        </nav-tabs-card>
      </div>
    </div>
  </div>
</template>

<script>
import {
  StatsCard,
  NavTabsCard,
  NavTabsTable,
  PendingOrdersTable,
  InventoryTable,
  IngredientPurchasesTable,
  RecipesTable,
  CompletedOrdersTable,
  Spinner,
  ThePagination
} from "@/components";
import axios from 'axios'

export default {
  components: {
    //StatsCard,
    NavTabsCard,
    PendingOrdersTable,
    InventoryTable,
    IngredientPurchasesTable,
    RecipesTable,
    CompletedOrdersTable,
    Spinner,
    ThePagination
  },
  async mounted() {
    this.getPendingOrders();
    this.getCompletedOrders();
    this.getPurchases();
    await this.getInventory();
    await this.getRecipes();
    setInterval(() => {
      this.getInventory();
      this.getPurchases();
    }, 20000);
  },
  methods: {
    async getInventory() {
      try {
        this.loadingInventory = true;
        const { data } = await axios.get(this.msInventoryUrl + '/inventory',{
          params:{
            take: 6,
            page:this.inventoryPagination.current_page
          }
        })
        this.inventoryIngredients = data.data;
        this.inventoryPagination = data.pagination;
        this.loadingInventory = false;
      } catch (err) {
        this.$alertify.error("Ocurrió un error al intentar hacer la solicitud");
        this.loadingInventory = false;
      }
    },
    async getRecipes() {
      try {
        this.loadingRecipes = true;
        const { data } = await axios.get(this.msRecipeUrl + '/recipes',{
          params:{
            take: 6,
            page:this.recipePagination.current_page
          }
        })
        this.recipesData = data.data;
        this.recipePagination = data.pagination;
        this.loadingRecipes = false;
      } catch (err) {
        this.$alertify.error("Ocurrió un error al intentar hacer la solicitud");
        this.loadingRecipes = false;
      }
    },
    async getPurchases() {
      try {
        this.loadingPurchases = true;
        const { data } = await axios.get(this.msInventoryUrl + '/purchases',{
          params:{
            take: 6,
            page:this.purchasesPagination.current_page
          }
        })
        this.ingredientPurchases = data.data;
        this.purchasesPagination = data.pagination;
        this.loadingPurchases = false;
      } catch (err) {
        this.$alertify.error("Ocurrió un error al intentar hacer la solicitud");
        this.loadingPurchases = false;
      }
    },
    async requestOrder() {
      try {
        this.loadingRequestOrder = true;
        const { data } = await axios.post(this.msKitchenUrl + '/orders');
        this.$alertify.success(data.data.message);
        this.loadingRequestOrder = false;
        this.getPendingOrders();
        this.getCompletedOrders();
      } catch (err) {
        this.$alertify.error("Ocurrió un error al intentar hacer la solicitud");
        this.loadingRequestOrder = false;
      }
    },//requestOrder
    async getPendingOrders() {
      try {
        this.loadingPendingOrders = true;
        const { data } = await axios.get(this.msKitchenUrl + '/orders', {
          params: {
            pending_orders: 1,
            order_direction: 'ASC',
            take: 6,
            page:this.pendingdOrdersPagination.current_page
          }
        });
        this.pendingOrders = data.data;
        this.pendingdOrdersPagination = data.pagination;
        this.loadingPendingOrders = false;
      } catch (err) {
        this.$alertify.error("Ocurrió un error al intentar hacer la solicitud");
        this.loadingPendingOrders = false;
      }
    },//getPendingOrders
    async getCompletedOrders() {
      try {
        this.loadingCompletedOrders = true;
        const { data } = await axios.get(this.msKitchenUrl + '/orders', {
          params: {
            status: "completed",
            order_direction: 'DESC',
            take: 6,
            page:this.completedOrdersPagination.current_page
          }
        });
        this.completedOrders = data.data;
        this.completedOrdersPagination = data.pagination;
        this.loadingCompletedOrders = false;
      } catch (err) {
        alert('Ocurrió un error al intentar hacer la solicitud');
        this.loadingCompletedOrders = false;
      }
    },//getCompletedOrders
  },
  data() {
    return {
      recipePagination: {
        current_page: 1,
        total: 0,
        last_page: 1,
      },
      inventoryPagination: {
        current_page: 1,
        total: 0,
        last_page: 1,
      },
      purchasesPagination: {
        current_page: 1,
        total: 0,
        last_page: 1,
      },
      completedOrdersPagination: {
        current_page: 1,
        total: 0,
        last_page: 1,
      },
      pendingdOrdersPagination: {
        current_page: 1,
        total: 0,
        last_page: 1,
      },
      loadingCompletedOrders: false,
      completedOrders: [],
      loadingPendingOrders: false,
      loadingRequestOrder: false,
      loadingInventory: false,
      inventoryIngredients: [],
      loadingRecipes: false,
      recipesData: [],
      loadingPurchases: false,
      ingredientPurchases: [],
      pendingOrders: [],
      msInventoryUrl: process.env.VUE_APP_INVENTORY_API_URL,
      msRecipeUrl: process.env.VUE_APP_RECIPES_API_URL,
      msKitchenUrl: process.env.VUE_APP_KITCHEN_API_URL,
    };
  },
};
</script>
