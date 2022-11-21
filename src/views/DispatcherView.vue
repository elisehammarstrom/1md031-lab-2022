<template>
    <div id="orders">
      <div id="orderList">
        <div v-for="(order, key) in orders" v-bind:key="'order'+key">
          #{{ key }}: <!-- {{ order.orderItems.join(", ") }} -->
          <div v-for="(amount, name) in order.orderItems" v-bind:key="'key'+key+name">
            Order: {{name}}. Amount: {{amount}}
            <!-- <div v-for="(fullName, email, payment, gender) in order.customer" v-bind:key="'key'+key+name+fullName"> 
              <div v-for= "(fullName, email, payment, gender) in order.customer.fullName" v-bind:key="'key'+key+name+fullName" v-bind:style="{color:'gray', font: '0.5em Arial, sans-serif;' }"> -->
               <div v-bind:style="{color:'gray' }"> 
              {{order.customer.fullName}}
              ({{order.customer.email}}, {{order.customer.payment}}, {{order.customer.gender}})

            </div>
          </div>
        </div>
        <button v-on:click="clearQueue">Clear Queue</button>
      </div>
      <div id="dots" v-bind:style="{ background: 'url(' + require('../../public/img/polacks.jpg')+ ')' }">
          <div v-for="(order, key) in orders" v-bind:style="{ left: order.details.x + 'px', top: order.details.y + 'px'}" v-bind:key="'dots' + key">
            {{ key }}

          </div>
      </div>
      
    </div>
  </template>
  <script>
  import io from 'socket.io-client'
  const socket = io();
  
  export default {
    name: 'DispatcherView',
    data: function () {
      return {
        orders: null
      }
    },
    created: function () {
      socket.on('currentQueue', data =>
        this.orders = data.orders);
    },
    
    methods: {
      clearQueue: function () {
        socket.emit('clearQueue');
      }
    }
  }
  </script>
  <style>
  #orderList {
    top:1em;
    left:1em;
    position: absolute;
    z-index: 2;
    color:black;
    background: rgba(255,255,255, 0.5);
    padding: 1em;
  }
  #dots {
    position: relative;
    margin: 0;
    padding: 0;
    background-repeat: no-repeat;
    width:1920px;
    height: 1078px;
    cursor: crosshair;
  }
  
  #dots div {
    position: absolute;
    background: black;
    color: white;
    border-radius: 10px;
    width:20px;
    height:20px;
    text-align: center;
  }
  </style>
  