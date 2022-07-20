<template>
  <div v-if="show" @click.stop="hideDialog" @click.escape="hideDialog" class="order modal">
    <div @click.stop  class="modal__content">
      <button @click.stop="hideDialog" class="modal__hide">
        <svg  x="0px" y="0px" width="20px" height="20px"
          viewBox="0 0 490 490" style="enable-background:new 0 0 490 490;" xml:space="preserve">
        <polygon points="11.387,490 245,255.832 478.613,490 489.439,479.174 255.809,244.996 489.439,10.811 478.613,0 245,234.161 
          11.387,0 0.561,10.811 234.191,244.996 0.561,479.174 "/>
        </svg>
      </button>
      <div class="order__title">Заказ №{{ modal.id }}</div>
      <p class="order__subtitle">Корзина</p>
      <div
        class="order__products"
        v-for="product in modalOrders"
        :key="product.id"
      >
        <div class="products__item product">
          <div class="product__name">{{ product.name }}</div>
          <div class="product__quantity">{{ product.quantity }} шт.</div>
          <div class="product__price">{{ product.price }} руб.</div>
        </div>
      </div>
      <div class="order__total">Итого: {{ modal.total }} руб.</div>
      <div class="order__wrapper">
        <button
          @click="addOrderToShipments"
          class="order__btn order__btn_color-green"
        >
          К отгрузке
        </button>
        <button @click="deleteOrder" class="order__btn order__btn_color-red">
          Отменить
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "modal-list",
  props: {
    modalOrders: {
      type: Array,
      required: true,
    },
    modal: {
      type: Object,
      required: true,
    },
    show: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {};
  },
  methods: {
    hideDialog() {
      this.$emit("update:show", false);
    },
    async addOrderToShipments () {
      await fetch(
        `https://demo-api.vsdev.space/api/orders_admin/0639/orders/${this.modal.id}/delivery`,
        {
          method: "POST",
        }
      ).then(() => {
        this.$emit("refreshShipmentList");
        this.hideDialog();
      });
    },
    async deleteOrder () {
      await fetch(
        `https://demo-api.vsdev.space/api/orders_admin/0639/orders/${this.modal.id}`,
        {
          method: "DELETE",
        }
      ).then(() => {
        this.$emit("refreshOrderList");
        this.hideDialog();

      });
    },
  },
};
</script>

<style lang="scss" scoped>
.modal {
  top: 0;
  bottom: 0;
  right: 0;
  left: 0;
  background: rgba(0, 0, 0, 0.2);
  position: fixed;
  z-index: 11;
  display: flex;
  &__content {
    margin: auto;
    background: white;
    min-height: 50px;
    min-width: 300px;
    padding: 20px;
    position: relative;
  }
  &__hide {
    background-color: #FFFFFF;
    border: none;
    position: absolute;
    right: 20px;
    cursor: pointer;
  }
}
.order {
  &__title {
    text-align: center;
    margin-bottom: 25px;
  }
  &__wrapper {
    display: flex;
    justify-content: space-around;
    margin-top: 20px;
  }
  &__btn {
    background-color: #ffffff;
    border: 2px solid teal;
    padding: 10px;
    font-size: 1rem;
    &_color-green {
      color: green;
    }
    &_color-red {
      color: red;
    }
  }
}
.product {
  display: flex;
  justify-content: space-between;
  border: 2px solid teal;
  margin: 10px 0;
  padding: 10px 20px;
  font-size: 1rem;
  &__name {
    margin-right: 40px;
  }
  &__quantity {
    margin-right: 20px;
  }
}
</style>