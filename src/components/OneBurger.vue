<template>
  
            <div class="box">
                <h3>{{burger.name}}</h3>
                     <img v-bind:src="burger.image" alt="Span" style="height: 200 px; width: 300px;">
                     <dl>
                <dd>{{burger.info}}</dd> 

                <dt v-if="burger.gluten" class="orange"> {{"jättemycket gluten"}} </dt>
                <dt v-if="burger.laktos" class="orange">{{"supermycket laktos"}} </dt>
                <dt>{{burger.burgerKcal}}</dt>
                <dt>{{amountOrdered}}</dt>
                <button type="button"  v-on:click="addBurger"> 
                  Mer burgare
                </button>
                <button type="button"  v-on:click="removeBurger"> 
                  Nä förresten
                </button>
                </dl>     
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
    
  }},
  methods: {
    

    addBurger: function () {
     
     this.amountOrdered += 1;
    this.$emit('orderedBurger', { name:   this.burger.name, 
                                amount: this.amountOrdered 
                              }
  );
},

removeBurger: function () {

  if (this.amountOrdered>0){
  this.amountOrdered -= 1;}
  this.$emit('orderedBurger', { name:   this.burger.name, 
                                amount: this.amountOrdered 
                              }
  );
},

   
}

// In child component



}


  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>
  .orange{

    color:orange
  }
  </style>
  