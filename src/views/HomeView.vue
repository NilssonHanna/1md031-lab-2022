

<template id="skärm">
  <div>
    <header class="header">
        
       
        <img  id="headerbild" 
         src="https://i0.wp.com/post.psychcentral.com/wp-content/uploads/sites/4/2022/06/buddha-statue-outdoors-sky-china-1296x728-header-1024x575.jpg?w=1155&h=1528" width="1045">
         <h1  id="headline"> Välkommen till The mindful burger</h1>
    </header>
   
    <main>
       <section id="meny">

          <h2>Välj hamburgare</h2>

          Det är här du väljer vilken hamburgare du vill ha.
          <br> Du måste välja åtminstone en;)
          
          

        <div class="wrapper">
            <Burger v-for="burger in burgers"
                v-bind:burger="burger" 
                v-bind:key="burger.name"
                v-on:orderedBurger="addToOrder($event)"
              />
          </div>     
           
         

       </section>

       <section id="leveransinformation"> 
        <h1> Kundinformation</h1>
       <dd> “When you bow, you should just bow; when you sit, you should just sit; when you eat, you should just eat."- Suzuki Roshi</dd>
        <h2> Leveransinformation</h2>
        
        <form>
            <p>
                <label for="firstname">Full name</label><br>
                <input type="text" id="first name" v-model="fn" required="required" placeholder="För- och efternamn">
            </p>
            
            <p>
                <label for="mail">Mailadress</label><br>
                <input type="email" id="mail" v-model="em" required="required" placeholder="E-mail address">
            </p>

        

            <p>
                <label for="Betalningsinformation">Betalmedel</label>
                <select id="Betalningsinformation" v-model="rcp">
                    <option>allmosa</option> 
                    <option>faktura</option>
                    <option selected="selected" >sedlar</option>
                    <option>sten</option>
                </select>
             </p>
        <fieldset>
           <p>
                <h4>Ange kön</h4>
                <input type="radio" id="flicka" v-model="kön" value="flicka" checked="checked">
                <label for="flicka">flicka</label><br>

                
                <input type="radio" id="pojke" v-model="kön" value="pojke" >
                <label for="pojke">pojke</label><br>

            
                <input type="radio" id="vill ej uppge" v-model="kön" required="vill ej uppge">
                <label for="vill ej uppge">vill ej uppge</label><br>
                
            </p>
        </fieldset>
       </form>

        </section>


       
       
          <div id="scrollbar">
            <div class="map" id="dots" v-on:click="setLocation">
              <div v-bind:style="{ left: this.location.x+ 'px', top: this.location.y + 'px'}">

                T
              </div>
              
             
            </div>
          </div>




        
        

       <button type="submit" id="läggorder" v-on:click="printlog" >
        <img src="http://www.buddhismen.se/wp-content/uploads/2016/01/DWBA9204_z9r8537_hun_rve-lpr.jpg"  style="width: 50px;">
        Lägg order
        
      </button>
      

     
    </main>
    <footer>
        <hr>
       &copy;Hannas hemsida!!! rör ej!
    </footer>


 



</div>



</template>



<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'

const socket = io();

function MenuItem(bn,im,inf,gl,lc,kCal) {
    this.name = bn; // The *this* keyword refers to the object itself
    this.image=im;
    this.info= inf;
    this.gluten = gl;
    this.laktos = lc;
    this.burgerKcal = kCal;
    }

   const firstBurger = new MenuItem("The takes forever to eat burger","https://idenyt-viivilla.imgix.net/sites/2/2021/09/hamburgare.jpg", "Det tar lite tid att äta upp 25 burgare. Ett jättebra sätt att vara närvarande i stunden!","jättemycket gluten","supermycket laktos",30+" kcal");
   const secondBurger = new MenuItem('The Buddha burger',"https://cdn77-s3.lazycatkitchen.com/wp-content/uploads/2017/03/vegan-bao-buns-single-800x1200.jpg","förmodligen Buddhas favoritburgare.",'glutenfree', 'lactosfree', 200+" kcal" );
   const thirdBurger = new MenuItem("The do it yourself burger","https://i2.wp.com/completelydelicious.com/wp-content/uploads/2014/07/IMG_7994.jpg","Gör det själv helt enkelt. Kul och meditativt!" , 'glutenfree', 'laktos',1000+" kcal",);
    


   const myBurgers=[firstBurger,secondBurger,thirdBurger];
  


export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    
    return {
      burgers:menu,
      fn:"",
      em:"",
      kön:"",
      rcp:"",

      orderedBurgers:{},

      location: { x: 0,
            y: 0
          }
    }
    
  },


  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100);
    },
     addToOrder: function (event) {
  this.orderedBurgers[event.name] = event.amount;
},
   

printlog: function(event){

      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: this.location.x,
                                           y: this.location.y,
                                          name:this.fn,
                                          email:this.em,
                                        gender:this.kön,
                                        payment:this.rcp
                                      },
                                  orderItems: this.orderedBurgers
                              }
                 );


      console.log(this.fn,this.em,this.ga,this.hn,this.kön,this.rcp,this.orderedBurgers,this.location)
      
    },

   

    
    
    addOrder: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};

                    this.location.x=event.clientX - 10 - offset.x
                    this.location.y=event.clientY - 10 - offset.y   
      
    },

    setLocation:function (event){

      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top,};

                    this.location.x=event.clientX - 10 - offset.x
                    this.location.y=event.clientY - 10 - offset.y 

    }



  }
}
</script>

<style>


#scrollbar{

  margin:auto;
  width:900px;
  height:550px;
  overflow:scroll;
  border:0; padding:0;
}

.map {

    width: 1920px;
    height: 1078px;
    background: url("/public/img/polacks.jpg"); 
    
    
  }

 

/*empty*/

.beskrivandetext{
    font-size: 50px;
}


.wrapper {
    display: grid;
    grid-gap: 100px;
    grid-template-columns: 300px 300px 300px;
    background-color: #885400;
    color: #e4b66c;

}

.box {
    background-color: rgb(136, 100, 31);
    color: rgb(255, 255, 255);
    border-radius: 5px;
    padding: 20px;
    font-size: 100%;
    width: 100%;
    margin-left: 70px;
    
}


#skärm {
margin-top: 100px;
margin-bottom: 100px;
width: 100%;
 
}

#läggorder {
margin-top: 20px;
}

#headline{
    font-family: cursive;
    margin-right: 155px;
    margin-left: 2px;
    position: absolute;
    color: #885400;
    padding: 250px 70px 75px 100px;
    margin-top: -600px;
    
    
}
#headerbild {
    opacity: 0.7;
    width: 100%;
    height:auto;
    
}
.header{ height: 20;

}

body {
    font-family: 'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Verdana, sans-serif;
    background-color: #885400; 
  }


 #meny {
    font-weight: bold;
    font-size: small;
    background-color: #885400;
    border:dashed;
    border-color: white;
    text-align: center;
   
    
 }
 
#leveransinformation {
    background-color: #000000;
    color:white;
    border:dashed;
    border-color: #885400;
    text-indent: 50px;
}
 #laktos {
    color: #ff5500;
 }
 #gluten {
    color: #ff5500;
 }

 button:hover {
    background-color: rgb(14, 111, 14);
    cursor: pointer;
    
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


</style>