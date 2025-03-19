# [Restaurant Mr. Tomato (VueJs)](https://github.com/sabaz120/vue-restaurant)

![Dashboard](https://i.ibb.co/cc5mCTCd/restaurant-mr-tomato.png)

Este frontend, desarrollado con Vue 2 utilizando una plantilla de creative Tim, proporciona la interfaz de usuario para la aplicación de donación de comida del restaurante. Permite al gerente del restaurante interactuar solicitar platos de comida, poder visualizar las órdenes en proceso y completadas, además de poder visualizar el inventario actual y el historial de compras (Cuando se agotan los ingredientes).

La aplicación consta de las siguientes secciones principales:

* **Pedidos:**
    * Permite al gerente realizar pedidos de platos a la cocina mediante un botón.
    * Muestra las órdenes en preparación y el historial de pedidos.
* **Inventario:**
    * Muestra los ingredientes disponibles y sus cantidades en la bodega de alimentos.
    * Muestra el historial de compras de ingredientes en la plaza de mercado.
* **Recetas:**
    * Muestra las recetas disponibles con sus ingredientes y cantidades.

## Tecnologías Utilizadas

* Vue 2
* Axios (para realizar peticiones HTTP a los microservicios)
* Vue Router (para la navegación de las vistas)

## Conexión con Microservicios

El frontend se conecta con los siguientes microservicios a través de sus APIs RESTful:

1.  **Microservicio de Gestión de la Cocina ([Kitchen Service](https://github.com/sabaz120/ms-kitchen)):**
    * API utilizada para realizar pedidos de platos y obtener el estado de las órdenes.
    * Endpoint base: `/api/orders`
2.  **Microservicio de Gestión de la Bodega ([Inventory Service](https://github.com/sabaz120/ms_inventory)):**
    * API utilizada para obtener el inventario de ingredientes y el historial de compras.
    * Endpoint base: `/api/inventory`
3.  **Microservicio de Recetas ([Recipe Service](https://github.com/sabaz120/ms_recipes)):**
    * API utilizada para obtener las recetas de los platos de comida.
    * Endpoint base: `/api/recipes`

## Funcionalidades Principales

* **Realizar pedidos:** El gerente puede presionar un botón para enviar una orden a la cocina.
* **Visualizar órdenes:** Se muestran las órdenes en preparación y el historial de pedidos.
* **Consultar inventario:** Se muestra la disponibilidad de ingredientes en la bodega.
* **Ver historial de compras:** Se muestra el registro de compras realizadas en la plaza de mercado.
* **Visualizar recetas:** Se muestran las recetas disponibles con sus ingredientes y cantidades.

## Configuración

1.  Asegúrate de que los microservicios estén en ejecución y accesibles.
2.  Configura las URLs de los microservicios en el archivo de configuración del frontend (Por defecto en el example, están con las URL'S y puertos correspondientes al ser levantados con el docker-compose).
3.  Instala las dependencias del frontend: `npm install`
4.  Ejecuta el frontend en modo de desarrollo: `npm run serve`

## Consideraciones

* Este frontend está diseñado para interactuar con los microservicios especificados.

Espero que este README sea útil para comprender el funcionamiento del frontend.
