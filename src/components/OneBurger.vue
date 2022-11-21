<template>
    <div>
      <div class="burger-box">
        <h2> {{ burger.name }} </h2>
        <img v-bind:src="burger.url" alt="Span" title="Fire Burger Image">
        <br />
        <div class="ingredients">
           <ul>
              <li id="vegan-friendly-fire-burger">Vegan option: {{ burger.vegan }} </li>
              <li id="ingredient">Calories: {{ burger.kCal }}</li>
              <li id="ingredient">Lactose: {{ burger.lactose}}</li>
              <li id="ingredient">Gluten:  {{burger.gluten}}</li>
           </ul> 
       </div>    
      </div>  
      Amount: 
      <button v-on:click="decreaseBurgerCount">-</button>
      {{ amountOrdered }} 
      <button v-on:click="addBurgerCount">+</button>
    
  <!-- 
    <h3>{{ burger.name }}</h3>
    {{burger.url}}
    <br>
    <ul>
      <li>Calories: {{ burger.kCal }} </li>
      <li>Lactose: {{ burger.lactose}} </li>
      <li>Gluten:  {{burger.gluten}} </li>
    </ul>
    <br>
      <button v-on:click="selectThisBurger">Select</button>
   -->   
      
    </div>

  </template>
  
  <script>
  export default {
    name: 'OneBurger',
    props: {
      burger: Object
    }, 


    data: function () {
      return {
        amountOrdered: 0,
      }
    },

    methods: {
    selectThisBurger: function() {
    this.$emit("selectedB", this.burger)
      },

    decreaseBurgerCount: function() {
      if (this.amountOrdered-1 <= 0) {
        this.amountOrdered = 0;
        console.log("Ordered burger:", this.burger.name, "is a cancelled order")
      }
      else if (this.amountOrdered-1 >= 0) {
        this.amountOrdered -= 1;
        //console.log("Ordered burger:", this.burger.name, ".", "Ordered amount:", this.amountOrdered, ".")
        this.$emit('orderedBurger', { name:   this.burger.name, 
                                      amount: this.amountOrdered 
                                    }
        );
      }
      
    },
    addBurgerCount: function() {
      this.amountOrdered += 1;
      //console.log("Ordered burger:", this.burger.name, ".", "Ordered amount:", this.amountOrdered, ".")
      this.$emit('orderedBurger', { name:   this.burger.name, 
                                    amount: this.amountOrdered 
                                  }
      );
      console.log("added this data to parent", this.burger.name, this.amountOrdered)
    }, 
    

    }


  }
  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>

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

    .ingredients li {
    color: #3a3b3c;
    font-family: DM Sans;
    font-weight: 700;
    
  }

  #vegan-friendly-fire-burger {
     text-transform: uppercase;
  }

  .burger-box > img {
  width: 100%;
}


.burger-box > h2 {
  text-align: center;
}

  
  </style>
  