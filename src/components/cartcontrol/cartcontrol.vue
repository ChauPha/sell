<template>
  <div class="cartcontrol">

    <div class="cart-decrease icon-remove_circle_outline" v-show="food.count>0" @click.stop.prevent="decreaseCart" transition="move">
      <span class="inner">-</span>
    </div>

    <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
    <div class="cart-add icon-add_circle" @click.stop.prevent="addCart">+</div>
  </div>
</template>

<script type="text/ecmascript-6">
  import Vue from 'vue';
  export default {
    props: {
      food: {
        type: Object
      }
    },
    created() {
    },
    methods: {
      addCart(event) {
        if (!event._constructed) {
          return;
        }
        if (!this.food.count) {
          Vue.set(this.food, 'count', 1);
        } else {
          this.food.count++;
        }
      },
      decreaseCart(event) {
        if (!event._constructed) {
          return;
        }
        if (this.food.count) {
          this.food.count--;
        }
      }
    }
  };
</script>

<style>
  .cartcontrol .cart-decrease{
      display:inline-block;
      padding:6px;
    }
    .cartcontrol .cart-decrease>.inner{
      display:inline-block;
      font-size:24px;
      color: rgb(0,160,220);
    }

    .cartcontrol .cart-count{
      display:inline-block;
    }
    .cartcontrol .cart-add{
      display:inline-block;
      padding:3px;
      width:10px;
      height:10px;
      line-height:10px;
      font-size:16px;
      border-radius:50%;
      color:#fff;
      background: rgb(0,160,220);
    }
    .cartcontrol .icon-remove_circle_outline{
      border:1px solid rgb(0,160,220);
      background:#fff;
      width:8px;
      height:8px;
      display:inline-block;
      padding:3px;
      border-radius:50%;
      margin-top:1px;
      line-height:6px;
      color:rgb(0,160,220);
    }
    .cart-count{
      display:inline-block;
      vertical-align:top;
      width:12px;
      padding-top:6px;
      line-height:6px;
      text-align:center;
      font-size:10px;
      color:rgb(147,153,139);
    }
</style>
