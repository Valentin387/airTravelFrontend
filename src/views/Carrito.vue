<template>
    <div class="cart-container">
      <div class="cart-header">
        <h2>Carrito de Compra</h2>
      </div>
      <div v-if="editingIndex === -1">
        <div class="cart-items">
          <table>
            <thead>
              <tr>
                <th class="left-align">Vuelo</th>
                <th class="left-align">Origen</th>
                <th class="left-align">Destino</th>
                <th class="left-align">Fecha de despegue</th>
                <th class="left-align">Cantidad de asientos</th>
                <th class="left-align">Costo por pasajero</th>
                <th class="left-align">Costo con Oferta</th>
                <th class="left-align"></th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(item, index) in cartItems" :key="index">
                <td class="left">{{ item.flightId }}</td>
                <td class="left">{{ item.origin }}</td>
                <td class="left">{{ item.destination }}</td>
                <td class="left">{{ formatDate(item.flightDate) }}</td>
                <td class="left">{{ item.seats.length }}</td>
                <td class="left" :class="{ 'strike-through': item.costByPersonOffer > 0 }">${{ item.costByPerson }}</td>
                <td class="left">${{ item.costByPersonOffer }}</td>
      
                
                <td>
                    <button class="button-delete" @click="removeItem(index)">X</button>
                </td>
                
                <td>
                  <button class="button-edit" @click="toggleEdit(index)">
                  <span class="material-symbols-outlined">edit</span>
                  </button>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
        <div class="cart-total">
          <p><strong>Total:  </strong> $ {{ total  }}</p>
          <button class="button-buy" @click="purchase">Comprar Ahora</button>
        </div>
      </div>
      
      <div v-else>
        <div class="edit-section">
          <h2>Edita Tu Reserva</h2>
          <label for="seatClass">Clase:</label>
          <select class="seatClass"  placeholder="Seleccione una clase" v-model="seatClass">
            <option value="" disabled selected>Seleccione una clase</option>
            <option value="First Class">Primera Clase</option>
              <option value="Economic Class">Clase Económica</option>
          </select>
          <br>
          <label for="seatQuantity">Cantidad de asientos:</label>
          <select Class="seatQuantity" v-model="seatQuantity">
            <option v-for="i in 5" :value="i" :key="i">{{ i }}</option>
          </select>
          <br>
          <button class="guardar" @click="saveChanges">Guardar</button>
        </div>
      </div>
    </div>
  
      <!------------------------------------------------FOOTER------------------------------------------->
  <Footer></Footer>
</template>
  
<style lang="scss" scoped>
    $light-color: #312c02;
  $degradado: rgba(39, 64, 153, 0.479);
  $bg: rgba(6, 31, 14, 0.873);
  $azul-claro: #cfe0eb;
  $gris: #f7f7f7;
  $gris2: #364265;
  $verde: #00bd8e;
  $azul: #0d629b;
  $blanco: #ffffff;
  $negro: #1a1320;
  $accent: #0b97f4;
  $accent3: #77797a;
  $blue: #54b2f1;
  $secondary: #ceeafd;
  $card: #0d629b17;

html {
    /* 
            Estilo CSS para la vista Home.vue del proyecto Vue.js. 
            La propiedad font-size establece el tamaño de fuente base en 10px, lo que equivale a 62.5% del tamaño de fuente predeterminado del navegador. 
            La propiedad overflow-x:hidden oculta el desplazamiento horizontal de la página. 
            La propiedad scroll-behavior: smooth agrega un efecto de desplazamiento suave al hacer clic en los enlaces internos de la página. 
            La propiedad scroll-padding-top establece la cantidad de espacio de relleno en la parte superior de la página para compensar la barra de navegación fija. 
            */
    font-size: 62.5%;
    overflow-x: hidden;
    scroll-behavior: smooth;
    scroll-padding-top: 9rem;

    /* 
            Estilos para la barra de desplazamiento en la vista Home.vue.
            Se utiliza el selector de pseudo-elemento &:: para aplicar estilos a la barra de desplazamiento.
            */
    &::-webkit-scrollbar {
        width: 1rem;
    }

    &::-webkit-scrollbar-track {
        background: $accent;
    }

    &::-webkit-scrollbar-thumb {
        background-color: $azul;
    }

}
.edit-section {
   margin:0 auto;
   text-align: center;
   h2{
    padding-bottom:2rem ;
   }

   margin-bottom: 1rem; // Espacio entre elementos centrados
  .guardar {
    width: 20%;
    padding: 5px;
    margin-top: 7px;
    background-color: $blue;
    color: #fff;
    border: none;
    border-radius: 3px;
    margin-bottom: 2rem;
    cursor: pointer;
  }

   .seatClass  {
    width: 20%;
    margin-top: 10rem;
    margin: 10px 0;
    padding: 5px;
    border: 1px solid #ccc;
    border-radius: 5px;
  }
  .seatQuantity  {
    width: 5%;
    margin-top: 10rem;
    margin: 10px 0;
    padding: 5px;
    border: 1px solid #ccc;
    border-radius: 5px;
  }
  label {
    padding-right:1.1rem;
    font-weight: bolder;
  }
    
}
    .cart-container {
      padding: 20px;
      border-radius: 5px;
      height: auto;
      width: 90vw;
      margin: 0 auto; /* Centrar horizontalmente */
      margin-top: 10rem; /* Centrar verticalmente */
      background-color: $secondary;
    }
    .strike-through {
      text-decoration: line-through;
    }
    .cart-header {
    text-align: left;
    padding: 10px;
    }

    .cart-items {
    background-color: #f2f2f283;
    padding: 10px;

    


      .button-edit {
        background-color: transparent;
        border: none;
        cursor: pointer;
      }
      .cart-items table {
        width: 100%;
        overflow-x: auto; // Agregar desplazamiento horizontal en pantallas pequeñas
        white-space: nowrap; // Evitar que los elementos se envuelvan en pantallas pequeñas
      }

      .button-edit span {
        background-color: transparent; // Establecer el fondo del icono como transparente
        color: $azul; // Color del ícono si es necesario
        font-size: 2.6rem; // Tamaño del ícono
      }
    }
    .left-align {
      font-weight: bolder;
    }

    table {
    width: 100%;
    border-collapse: collapse;
    }

    th, td {
    padding: 8px;
    text-align: center; 
    }

    .flight{
        text-align: left;
    }

    .left-align {
    text-align: center; 
    }

    .button-delete  {
      background-color: rgb(212, 8, 8);
      color: white;
      border-radius: 25px;
      width: 25px;
      height: 25px;
      text-align: center;
      cursor: pointer;
      margin-left: auto;

    }
  .button-delete:hover {
    background-color: rgb(255, 0, 0);
  }

    .cart-total {
    display: inline;
    text-align: right;
    padding: 10px;
    p{
      font-size: 2rem;
    }
 
    }

    .button-buy {
    background-color: #1c88cf;
    color: white;
    border: none;
    padding: 10px 20px;
    border-radius: 5px;
    cursor: pointer;
    }
    // Media query para tamaños de pantalla más pequeños (ajusta según tus necesidades)
    @media (max-width: 768px) {
      .cart-items table {
        // Estilos específicos para tamaños de pantalla más pequeños
        font-size: 12px; // Tamaño de fuente más pequeño para adaptarse al espacio
      }
    }
</style>

<script>
import errorModal from "@/components/errorModal.vue";
import modifyService from '@/services/shoppingCartServices/modifyItemService.js';
import spinner from "@/components/spinner.vue";
import Footer from "@/components/footer.vue";
import successModal from "@/components/successModal.vue";
import listService from "@/services/shoppingCartServices/listService.js";
import checkoutService from "@/services/shoppingCartServices/checkoutService.js";
import dropService from "@/services/shoppingCartServices/dropService.js";

import { roundToNearestMinutes } from "date-fns";

export default {
  data() {
    return {
      flight: {},
      // ... (otras variables de tu data)
      cartItems: [],
      total: 0, // You may need to initialize this based on your requirements
      showSpinner: false, 
      editingIndex: -1,
      seatQuantity: 0,
      seatClass: "",
      flightId: 0,
      userID: 0,
      
    };
  },
  components: {
    errorModal,
    spinner,
    Footer,
    successModal,
  },
  
  mounted() {
    // Call the listShoppingCartItems function when the component is created
    this.listItems();
    this.getTotal();

    // You can also call other functions or perform additional logic here
  },
  methods: {
    
    async getTotal(){
      this.showSpinner = true;
      const token = window.sessionStorage.getItem('JWTtoken');
      const tokenData = JSON.parse(atob(token.split('.')[1]));
      const userID = tokenData.ID;

      // Llamar al servicio para obtener los items
      checkoutService.checkoutShoppingCart({"userID" : userID})
      
          .then(response => {
            this.showSpinner = false;
            if (response.status == 200){
              this.total = response.data.totalAmount;
              window.sessionStorage.setItem('total', JSON.stringify(this.total));
              console.log(response.data.totalAmount);
            }
          })
          .catch(error => {
            this.showSpinner = false;
            console.error(error);
          });
    },


    async listItems() {
      this.showSpinner = true;
      const token = window.sessionStorage.getItem('JWTtoken');
      const tokenData = JSON.parse(atob(token.split('.')[1]));
      const userID = tokenData.ID;

      // Llamar al servicio para obtener los items
      listService.listShoppingCartItems({"userID" : userID})
          .then(response => {
            this.showSpinner = false;
            if (response.status == 200){
              this.cartItems = response.data;
              window.sessionStorage.setItem('cartItems', JSON.stringify(this.cartItems));
              console.log(response);
            }

          })
          .catch(error => {
            this.showSpinner = false;
            console.error(error);
          });
    },
    

    async purchase(){
      this.showSpinner = true;
      //push to /Purchase
      this.$router.push("/Purchase");
    },

    removeItem(index) {
      const shouldDelete = window.confirm('¿Estás seguro de que deseas eliminar este elemento del carrito?');
      
      if (shouldDelete) {
        const token = window.sessionStorage.getItem('JWTtoken');
        const tokenData = JSON.parse(atob(token.split('.')[1]));
        const userID = tokenData.ID;

        dropService.dropItem({"userID" : userID, "flightID" : this.cartItems[index].flightId})
          .then(response => {
            if (response.status == 200){
              console.log(response.data);
              // Eliminar el elemento del carrito después de confirmar la eliminación
              this.cartItems.splice(index, 1);
              // Volver a calcular el total y la lista de elementos después de eliminar
              this.listItems();
              this.getTotal();
            }
          })
          .catch(error => {
            console.error(error);
          });
      } else {
        // Si el usuario cancela la eliminación, no se hace nada
        console.log('Eliminación cancelada');
      }
    },

    

    formatDate(dateString) {
     //Cambia el formato de la fecha de milisegundos a años, meses y dias
     const options = { year: "numeric", month: "long", day: "numeric" };
     const date = new Date(dateString);
     return date.toLocaleDateString("es-ES", options);
   },

   toggleEdit(index) {
      this.editingIndex = index;
      // Al hacer clic en editar, inicializar los valores con los actuales del elemento del carrito
      this.seatQuantity = this.cartItems[index].seats.length;
      this.seatClass = this.cartItems[index].class; // Supongamos que 'class' es la propiedad que contiene la clase de asiento
    },

    saveChanges() {
      const editedItem = this.cartItems[this.editingIndex];
      // Actualizar la información del elemento del carrito con los valores editados
      editedItem.seats = new Array(this.seatQuantity); // Actualizar la cantidad de asientos
      editedItem.class = this.seatClass; // Actualizar la clase de asiento

      // Enviar la solicitud para modificar el elemento del carrito
      const token = window.sessionStorage.getItem('JWTtoken');
      const tokenData = JSON.parse(atob(token.split('.')[1]));
      const userID = tokenData.ID;

      modifyService.modifyItemInCart(
        userID,
        editedItem.flightId, // Suponiendo que 'flightId' es la propiedad que identifica el vuelo
        this.seatQuantity,
        this.seatClass
      )
        .then(response => {
          // Manejar la respuesta del servidor si es necesario
          console.log("Elemento del carrito modificado:", response);
          // Después de modificar, resetear el estado de edición
          this.editingIndex = -1;
          this.getTotal();
        })
        .catch(error => {
          console.error("Error al modificar el elemento del carrito:", error);
          // Manejar errores si es necesario
        });
    },

    // Carrito.vue





  },
};

</script>

  