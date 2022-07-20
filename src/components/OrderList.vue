<template>
  <div class="orders">
    <div class="orders__header">
      <div @click="refreshOrderList" class="orders__refresh">
        <svg x="0px" y="0px"
                    width="25px" height="25px" fill="teal" viewBox="0 0 92.33 92.33" style="enable-background:new 0 0 92.33 92.33;" xml:space="preserve"
                    >
                <g>
                    <path d="M70.598,16.753c-1.722-1.24-4.113-0.852-5.349,0.866c-1.242,1.716-0.853,4.113,0.865,5.35
                        c13.613,9.818,18.021,27.857,10.482,42.89c-4.082,8.138-11.088,14.202-19.726,17.066c-8.636,2.871-17.877,2.2-26.013-1.879
                        c-8.134-4.083-14.197-11.088-17.066-19.722c-2.866-8.642-2.197-17.877,1.886-26.014c4.958-9.89,14.458-16.779,25.413-18.429
                        c0.074-0.008,0.137-0.036,0.211-0.053l0.157,7.571c0.021,0.839,0.542,1.585,1.321,1.889c0.782,0.305,1.672,0.11,2.25-0.496
                        l10.904-11.379c0.794-0.828,0.764-2.142-0.062-2.933L44.492,0.577c-0.606-0.582-1.499-0.739-2.267-0.399
                        c-0.251,0.108-0.476,0.269-0.662,0.462c-0.372,0.389-0.585,0.919-0.579,1.479l0.151,7.212c-0.385-0.063-0.78-0.087-1.188-0.027
                        c-13.418,2.021-25.052,10.46-31.125,22.571C-1.499,52.451,6.85,77.584,27.424,87.901c5.989,3.005,12.362,4.429,18.646,4.429
                        c15.306,0,30.065-8.439,37.382-23.028C92.688,50.884,87.284,28.782,70.598,16.753z"/>
                </g>
        </svg>
      </div>
      <p class="orders__title">Заказы</p>
    </div>
    <div class="orders__body" v-for="order in orders" :key="order.id" draggable="true" @dragstart="startDrag(order.id)">
      <div class="orders__item order">
        <div class="order__number">Заказ № {{ order.id }}</div>
        <div class="order__wrapper">
          <div class="order__total">{{ order.total }} руб.</div>
          <order-action-list 
            :order_id="order.id" 
            @openModal="openModal" 
            @addOrderToShipments="addOrderToShipments" 
            @deleteOrder='deleteOrder'
            class="order__options"
          >
          </order-action-list>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import OrderActionList from '@/components/OrderActionList.vue'
export default {
    name: 'order-list',
    props: {
        orders: {
            type: Array,
            required: true
        },
        shipments: {
            type: Array,
            required: true
        }
    },
    components: {
        OrderActionList
    },
    data () {
        return {
          
        }
    },
    methods: {
      refreshOrderList () {
        this.$emit("refreshOrderList");
      },
      refreshShipmentList () {
        this.$emit("refreshShipmentList");
      },

      async getOrderDetails (order_id) {
        const response = await fetch (`https://demo-api.vsdev.space/api/orders_admin/0639/orders/${order_id}`)
        return response.json();
      },
      openModal (order_id) {
        this.getOrderDetails(order_id).then((data) => {
          this.modal = data;
          this.$emit("openModal", this.modal);
        });
      },
      async addOrderToShipments (order_id) {
        await fetch(`https://demo-api.vsdev.space/api/orders_admin/0639/orders/${order_id}/delivery`, {
          method: 'POST',
        }).then(() => {this.refreshShipmentList()});
      },
      async deleteOrder(order_id) {
        await fetch(`https://demo-api.vsdev.space/api/orders_admin/0639/orders/${order_id}`, {
          method: 'DELETE',
        }).then(() => {this.refreshOrderList(); console.log(order_id)});
      },
      startDrag (order_id) {
        this.$emit("orderDragged", order_id);
      },


    }
}
</script>

<style lang="scss" scoped>
.orders {
  margin: 5px 25px 40px 25px;
  border: 2px solid teal;
  font-size: 1.5rem;
  &__header {
    display: flex;
    align-items: center;
  }
  &__title {
    margin: auto;
    padding-right: 25px;
    font-size: 1.25rem;
    font-weight: 600;

  }
  &__refresh {
    position: relative;
    top: 20px;
    left: 20px;
    border: 2px solid teal;
    padding: 5px;
  }
  &__item{
    padding: 10px;
    margin: 30px 50px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    border: 2px solid teal;
  }
}
.order {
  &__wrapper {
    display: flex;
    align-items: center;
    margin: 0 25px;
  }
  &__total {
    margin-right: 20px;
    font-size: 1rem;
  }
  &__number {
    font-weight: 600;
    font-size: 1.2rem;
  }
}

</style>