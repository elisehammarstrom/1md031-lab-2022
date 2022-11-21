<template>

  

  <div>
    <div>
    <header class="burger-header">
      <h1 id="siteintro">Welcome to BurgerHeaven Online</h1>
    </header>   
    <main>
      <section id="select-burger-section">
         <h2>Select burger </h2>
         <p>This is where you execute burger selection</p>
         <section id="burger-presentation-area">
          <br />
          <h2 id="burgerMenuId">Burger Menu</h2>
           <div class="wrapper">
                <!-- <Burger v-for="burger in burgers"
                        v-bind:burger="burger" 
                        v-on:selectedB="setSelectedBurger($event)"
                        v-bind:src="burger.url"
                        v-bind:key="burger.name"
                        v-on:orderedBurger="addToOrder($event)"
                        /> -->
                    <Burger v-for="burger in burgers"
                            v-bind:burger="burger" 
                            v-bind:key="burger.name"
                            v-on:orderedBurger="addToOrder($event)"/>    




            </div>    
        </section>
      </section>
      <section id="place-order-section">
        <h2>Customer information</h2>
        <p>This is where you provide necessary information </p>
        <section id="contact">
          <form>
            <p>
              <label for="fullname">Full name</label><br>
              <input type="text" id="fullname" v-model="formdata.fullName" required="required" placeholder="First- and Last name">
            </p>
            <p>
              <label for="email">Email</label><br>
              <input type="email" id="email" v-model="formdata.email" required="required" placeholder="E-mail address">
            </p>

            <!-- 
            <p>
              <label for="street">Street</label><br>
              <input type="text" id="street" v-model="formdata.street" placeholder="Street name">
            </p>
            <p>
              <label for="house">House</label><br> 
              <input type="number" id="house" v-model="formdata.houseNumber" placeholder="House number">
            </p>
          -->
            <div class="radiobuttons">
              <div class="singleRadioButton">
                <input type="radio" id="woman" value="Woman" v-model="formdata.genderIdentity" >
                <label for="woman">Woman</label>
              </div>
              <div class="singleRadioButton">
                <input type="radio" id="man" value="Man" v-model="formdata.genderIdentity" >
                <label for="Man">Man</label>
              </div>
              <div class="singleRadioButton">
                <input type="radio" id="non-binary" value="Do not wish to provide" v-model="formdata.genderIdentity" checked>
                <label for="non-binary">Do not wish to provide</label>
              </div>
            </div>
            <br />
            <br />
              <label for="paymentmethod" id="choose-paymentmethod-label">Choose your payment method:</label>
              <br />
              <select id="paymentmethod" v-model="formdata.paymentMethod">
                <option>Card</option>
                <option>Swish</option>
                <option>Banktransfer</option>
                <option selected="selected">Burger Heaven Customer Points</option>
                <option>Credit</option>
              </select>
            <br />
            <br />
            <div class="mapWrapper">
              <!-- <div id="map" v-on:click="addOrder" > -->
              <div id="map" v-on:click="setLocation($event)" > 

                <div id="dots" v-bind:style="{position:'relative', background: 'url(' + require('../../public/img/polacks.jpg')+ ')' }">
                 <!--  <div id="dots" v-bind:style="{ background: 'url(' + require('../../public/img/polacks.jpg')+ ')'}"> -->  
                    <div v-bind:location="location" v-bind:style=
                    "{ left: this.location.x + 'px', top: this.location.y + 'px', position: 'absolute'}" v-bind:key="'dots'">
                      T
                    </div>
                </div>
              </div>
             



            </div>  
            
            <br />

             <!-- <button v-on:click="placeOrder" id="place-order-button" type="submit" value="Place order"> -->
              <button v-on:click="placeOrder()" id="place-order-button" type="submit" value="Place order">
               <img src="hamburger.png" width="20px">
               <br>
                Place Order
            </button>
          </form>
          <br />


        </section>
      </section>


    </main>



    <!-- 
    Current delivery time is: {{deliveryTime}}
    
    <p>Select Your Burger</p> 
    You have added:
    {{selectedBurgers}}
    <br>

    <h2>Burger Menu</h2>
    <Burger v-for="burger in burgers"
            v-bind:burger="burger" 
            v-on:selectedB="setSelectedBurger($event)"
            v-bind:key="burger.name"/>
    </div>

    <button v-on:click="checkDeliveryTime">Check delivery time</button>


    <button v-for="btn in colorList" v-bind:style="{'background-color': btn.color}">
    {{btn.label}} </button>

    <div id="map" v-on:click="addOrder">
      click here
    -->

     


    
    </div>
  </div>




</template>

<script>

import Burger from '../components/OneBurger.vue' 
import io from 'socket.io-client'
import menu from '../assets/menu.json'

const socket = io();



function MenuItem(name, lactose, gluten, kCal, url, vegan) {
    this.name = name;
    this.kCal = kCal;
    this.url = url;
    this.vegan = vegan;
    this.lactose = lactose;
    this.gluten = gluten;
}

//burger Array
//var myBurgerOne = new MenuItem(menu.name, menu.lactose, menu.gluten, menu.kCal, menu.url, menu.vegan)
//var myBurgerTwo = new MenuItem(menu.name, menu.lactose, menu.gluten, menu.kCal, menu.url, menu.vegan)
//var myBurgerThree = new MenuItem(menu.name, menu.lactose, menu.gluten, menu.kCal, menu.url, menu.vegan)

//var myBurgerTwo = new MenuItem("The Vegan Burger", true, false, "900kCal", "vegan_burger.jpg", true)
//var myBurgerThree = new MenuItem("The Lettuce Wrapped Burger", false, true, "600kCal", "lettuce-wrapped-burger.jpg", true)

//let myBurgerArray = [myBurgerOne, myBurgerTwo, myBurgerThree]
//console.log(myBurgerArray)


//Home component
export default {
  name: 'HomeView',
  components: {
    Burger
  },

  created: function () {
    socket.on('deliveryTimeIs', time =>
      this.deliveryTime = time);
  },

  data: function () {
    return {
    deliveryTime: 0,
    orderedBurgers: {} ,
    formdata:{ fullName: '', email: '', genderIdentity: '', paymentMethod: 'Burger Heaven Customer Points'},
    formInfo: "förnamn", 
    //selectedBurgers: ["här är en burgare", "här är en annan burgare"],
    selectedBurgers: [],
    //burgers: myBurgerArray,
    burgers: menu,
      //burgers: [ {name: "small burger", kCal: 250},
       //          {name: "standard burger", kCal: 450},
       //          {name: "large burger", kCal: 850}
        //       ],
    
    location: { x: 0,
                y: 0
              },

    colorList: [
    {color: "#FF0000", label: "Röd"}, 
    {color:"#0000FF", label: "Blå"}, 
    {color:"ffeedd", label:"Gredelin"}
    ]

    }
  },

  methods: {

    setLocation: function(event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};

      this.location.x = event.clientX-10-offset.x;
      this.location.y = event.clientY-10- offset.y ;
      console.log("setLocation function runs")
      console.log(this.location.x);
      console.log(this.location.y);

    },

    addToOrder: function (event) {
      this.orderedBurgers[event.name] = event.amount;
      this.formdata;
    },

  placeOrder: function() {
    console.log("Placing order")
    console.log("formdata:")
    console.log(this.formdata)

    socket.emit("addOrder", { orderId: this.getOrderNumber(),
                              details: { x: this.location.x,
                                         y: this.location.y},
                              orderItems: this.orderedBurgers, 
                              customer: {
                                fullName: this.formdata.fullName, 
                                email: this.formdata.email,
                                payment: this.formdata.paymentMethod,
                                gender: this.formdata.genderIdentity
                              },
                              customerFullName: this.formdata.fullName, 

                            }
               );
    console.log("My object: " , this.orderedBurgers);
    console.log("My customer object: ", this.formdata)


  },   
  setSelectedBurger: function(burgare) {
    this.selectedBurgers.push(burgare.name);
  },
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },

    checkDeliveryTime: function () {
      socket.emit("deliveryTime");

    }, 


    addOrder: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: event.clientX - 10 - offset.x,
                                           y: event.clientY - 10 - offset.y },
                                orderItems: [this.orderedBurgers],
                                customerData: [this.formdata]
                              }
                 );
      this.location.x = event.clientX;
      this.location.y = event.clientY;


    }
  }
}
</script>

<style>

  @import url('https://fonts.googleapis.com/css2?family=DM+Sans:wght@400;700&family=DM+Serif+Display&display=swap');

  p, li, button, div {
    font: 1em DMSans-Regular, Helvetica, sans-serif;

  }
  h1 {
    font: 2em DMSerifDisplay-Regular, Helvetica, sans-serif;
  }
  h2 {
    font: 1.5em DMSerifDisplay-Regular, Helvetica, sans-serif;
  }
  h3 {
    font: 1.25em DMSerifDisplay-Regular, Helvetica, sans-serif;
  }

  #vegan-friendly-fire-burger {
     text-transform: uppercase;
  }

  button:hover {
     background: darksalmon;
  }

  section {
    margin: 10px;
    padding: 10px;
  }

  #siteintro {
    /* text-align: center; */
  }

  #select-burger-section {
    border: 2px dashed #000000;
    background-color: darksalmon;
  }

  #place-order-section {
    border: 2px dashed #000000;
    padding: 50px;
    background-color: #D7EBBA;
  }

  form {
    margin: -20px;
  }

  #place-order-button {
    /* place button in center */
    width: 100px;
    height: 70px;
    padding: 0px;
    margin: 20px;
    cursor: wait;
  }

  #paymentmethod {
    margin: -20px;
  }

  #choose-paymentmethod-label {
    margin: -20px;
  }

  .burger-header {
    margin: 10px;
    overflow: hidden;

  }

  .burger-header > h1 {
    position: absolute;
    margin-top: -700px;
    margin-left: 500px;
    color: #472836;
    background: darksalmon;
    border: 3px double #FEFFBE;
    border-radius: 50px;
    padding: 20px;
  }

  .burger-header > img {
    opacity: 0.6;
    width:  100%;
    height: auto;
  }

  .wrapper {
       display: grid;
       grid-gap: 5px;
       grid-template-columns: 300px 300px 300px;
       
   }

  .burger-box > h2 {
  text-align: center;
  }

  #burger-presentation-area {
    background-color: #FEFFBE;
    color: #444;
    border:  0px;
    border-radius: 100px;
    padding: 20px 0px 20px 20px;

  }

  #burger-presentation-area > h2 {
    text-align: center;
  }

  .mapWrapper{
    width: 1000px;
    height:  500px;
    overflow: scroll;

  }

  #map {
    width: 200%;
    height: 200%;
    /* background-color: red; */
    background: url("../../public/img/polacks.jpg");
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