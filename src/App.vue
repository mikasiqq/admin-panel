<template>
  <div id="app">
    <header class="header">
      <h1>Панель администратора заказов на Vue</h1>
    </header>
    <main class="main">
      <order-list
        :orders="orders"
        :shipments="shipments"
        @openModal="openModal"
        @refreshOrderList="refreshOrderList"
        @refreshShipmentList="refreshShipmentList"
        @orderDragged="orderDragged"
        class="section"
      >
      </order-list>
      <modal-list
        v-model:show="modalVisible"
        :modal="modal"
        :modalOrders="modalOrders"
        @refreshOrderList="refreshOrderList"
        @refreshShipmentList="refreshShipmentList"
      >
      </modal-list>
      <shipments-list
        :shipments="shipments"
        @refreshShipmentList="refreshShipmentList"
        @drop="toDrop"
        @dragover.prevent
        @dragenter.prevent
        class="section"
      >
      </shipments-list>
    </main>
    <footer class="footer">
      <p class="footer__name">Слинин Глеб Алексеевич, 211-321</p>
      <button @click="scroll" class="footer__scroll">
        <svg
          x="0px"
          y="0px"
          width="35px"
          height="35px"
          viewBox="0 0 1024 1024"
          class="icon"
          fill="teal"
        >
          <path
            fill="teal"
            d="M572.235 205.282v600.365a30.118 30.118 0 11-60.235 0V205.282L292.382 438.633a28.913 28.913 0 01-42.646 0 33.43 33.43 0 010-45.236l271.058-288.045a28.913 28.913 0 0142.647 0L834.5 393.397a33.43 33.43 0 010 45.176 28.913 28.913 0 01-42.647 0l-219.618-233.23z"
          />
        </svg>
      </button>
    </footer>
  </div>
</template>
<script>
import OrderList from "@/components/OrderList.vue";
import ModalList from "@/components/ModalList.vue";
import ShipmentsList from "@/components/ShipmentsList.vue";
export default {
  components: {
    OrderList,
    ModalList,
    ShipmentsList,
  },
  data() {
    return {
      orders: [],
      modalOrders: [],
      modal: [],
      shipments: [],
      isVisible: false,
      modalVisible: false,
      dragged: "",
    };
  },
  methods: {
    async getOrders() {
      const response = await fetch(
        "https://demo-api.vsdev.space/api/orders_admin/0639/orders",
        {
          method: "GET",
        }
      );
      this.orders = await response.json();
      console.log(this.orders);
    },
    async getShipments() {
      const response = await fetch(
        "https://demo-api.vsdev.space/api/orders_admin/0639/deliveries",
        {
          method: "GET",
        }
      );
      this.shipments = await response.json();
      console.log(this.shipments);
    },
    refreshOrderList() {
      this.orders = [];
      this.getOrders();
    },
    refreshShipmentList() {
      this.shipments = [];
      this.getShipments();
    },
    openModal(modal) {
      this.modalVisible = !this.modalVisible;
      this.modal = modal;
      this.modalOrders = modal.basket_items;
    },
    scroll() {
      document.documentElement.scrollIntoView({ smooth: true });
    },
    orderDragged(order_id) {
      this.dragged = order_id;
    },
    async toDrop() {
      await fetch(
        `https://demo-api.vsdev.space/api/orders_admin/0639/orders/${this.dragged}/delivery`,
        {
          method: "POST",
        }
      ).then(() => {
        this.refreshShipmentList();
      });
    }
  },

  mounted() {
    this.getOrders();
    this.getShipments();
    window.addEventListener("keydown", (e) => {
      if (e.key == "Escape") {
        this.modalVisible = false;
      }
    });
  },
  
};
</script>
<style lang="scss">

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: "Montserrat", sans-serif;
}
h1 {
  text-align: center;
}
.header {
  border: 2px solid teal;
  margin: 10px 25px;
  padding: 40px;
}
.main {
  display: flex;
  min-height: 150px;
  @media (max-width: 1000px) {
    flex-direction: column;
  }
}

.section {
  flex: 1;
}

.footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin: 0 25px;
  border: 2px solid teal;
  padding: 20px;
  &__scroll {
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: #ffffff;
    border: 2px solid teal;
  }
}
</style>
