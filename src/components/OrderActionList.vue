<template>
  <button class="orders__btn" @click="changeVisibility">
    <svg
      x="0px"
      y="0px"
      width="20px"
      height="20px"
      fill="teal"
      viewBox="0 0 30 30"
      style="enable-background: new 0 0 30 30"
      xml:space="preserve"
    >
      <g>
        <g>
          <path
            d="M4.01,10.99C1.795,10.99,0,12.786,0,15c0,2.215,1.795,4.01,4.01,4.01c2.215,0,4.01-1.795,4.01-4.01
                      C8.02,12.786,6.225,10.99,4.01,10.99z M4.01,17.229c-1.229,0-2.228-1-2.228-2.229c0-1.229,0.999-2.228,2.228-2.228
                      S6.238,13.771,6.238,15C6.238,16.229,5.238,17.229,4.01,17.229z"
          />
          <path
            d="M15,10.99c-2.215,0-4.01,1.795-4.01,4.01s1.795,4.01,4.01,4.01s4.01-1.795,4.01-4.01C19.01,12.786,17.215,10.99,15,10.99z
                      M15,17.229c-1.229,0-2.228-1-2.228-2.229c0-1.229,0.999-2.228,2.228-2.228c1.229,0,2.229,0.999,2.229,2.228
                      C17.229,16.229,16.229,17.229,15,17.229z"
          />
          <path
            d="M25.99,10.99c-2.215,0-4.01,1.795-4.01,4.01s1.795,4.01,4.01,4.01S30,17.215,30,15C30,12.786,28.205,10.99,25.99,10.99z
                      M25.99,17.229c-1.229,0-2.227-1-2.227-2.229c0-1.229,0.998-2.228,2.227-2.228s2.228,0.999,2.228,2.228
                      C28.218,16.229,27.218,17.229,25.99,17.229z"
          />
        </g>
      </g>
    </svg>
  </button>
  <div v-show="isVisible" class="order__options options">
    <ul class="options__list">
      <li class="options__item" @click="openModal">Подробнее</li>
      <li class="options__item options__item_color-green" @click="addOrderToShipments">К отгрузке</li>
      <li class="options__item options__item_color-red" @click="deleteOrder">Отменить</li>
    </ul>
  </div>
</template>

<script>
export default {
    name: 'order-action-list',
    props: ['order_id'],
    data () {
        return {
            isVisible: false
        }
    },
    methods: {
      changeVisibility: function() {
        this.isVisible = !this.isVisible;
      },
      openModal () {
        this.$emit("openModal", this.order_id);
        this.changeVisibility();
      }, 
      addOrderToShipments () {
        this.$emit("addOrderToShipments", this.order_id);
        this.changeVisibility();
      },
      deleteOrder () {
        this.$emit("deleteOrder", this.order_id);
        this.changeVisibility();
      },

    }
    
};
</script>

<style lang="scss" scoped>
.orders__btn {
    background-color: #FFFFFF;
    border: 2px solid teal;
    padding: 5px;
}
.options {
  position: relative;
  z-index: 10;
  background-color: #FFFFFF;
  padding: 0;
  &__list {
    position: absolute;
    right: 0;
    top: calc(100% + 20px);
    background-color: #FFFFFF;
    border: 2px solid teal;


  }
  &__item {
      list-style: none;
      cursor: pointer;
      padding: 5px;
      border-bottom: 2px solid teal;
      font-size: 1rem;
      &_color-red {
        color: red;
      }
      &_color-green {
        color: green;
      }
  }
  &__item:last-child {
    border-bottom: none;
  }
  
}
</style>