<template>
    <div class="page-container">
        <div class="left-content">
            <img class="imageReset-container" alt="" />
            <div class="left-content-inner">
                <hr />
                <h1 class="tittle">Aquí Puedes Editar Tus vuelos</h1>
                <hr />
                <div class="text-login">
                <p>
                    Somos tu pasaporte hacia el mundo. Con una amplia
                    gama de destinos, ofertas irresistibles y un servicio excepcional,
                    estamos aquí para hacer realidad tus sueños de viaje.
                </p>
                <hr />
                <hr />
                <p class="text1">
                    ¡Prepara tus maletas y comencemos a explorar el mundo juntos!
                </p>
                </div>
            </div>
            </div>
        <div class="login-container"> 
            <h1>Editar Carrito</h1>
            <div class="edit-form">
                <br>
                <p><strong>Vuelo:</strong>  {{ flightInfo.flightId }}</p>
                <p><strong>Origen:</strong> {{ flightInfo.origin }}</p>
                <p><strong>Destino:</strong> {{ flightInfo.destination }}</p>
                <p><strong>Fecha de salida:</strong>   {{ formatDate(flightInfo.flightDate) }}</p>
                <p><strong>Antes:</strong> $ {{ flightInfo.costByPerson }}</p>
                <p><strong>Ahora:</strong> ${{ flightInfo.costByPersonOffer }}</p>
               
                <label for="seatQuantity">Cantidad de Asientos:</label>
                <select v-model="seatQuantity" id="seatQuantity">
                <option value="1">1 </option>
                <option value="2">2 </option>
                <option value="3">3 </option>
                <option value="4">4 </option>
                <option value="5">5 </option>
                </select>
                <br>
                <label for="seatClass">Clase:</label>
                <select v-model="seatClass" id="seatClass">
                <option value="Economica">Económica</option>
                <option value="PrimeraClase">Primera Clase</option>
                </select>
        
                <button class="guardar" @click="updateCart">Guardar Cambios</button>
            </div>
        </div>
      
    </div>
  </template>
  

<style lang="scss" scoped >

$light-color: #2e2b18;
$degradado: rgba(34, 33, 39, 0.552);
$bg: rgba(25, 32, 57, 0.947);
$azul-claro: #cfe0eb;
$gris: #f7f7f7;
$verde: #00bd8e;
$azul: #0d629b;
$blanco: #ffffff;
$negro: #1a1320;
$blue: #54b2f1;
$accent: #0b97f4;
$accent2: #007bff;
$gris2: #364265;
$accent3: rgba(149, 168, 238, 0.11);
$secondary: #ceeafd;
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

.page-container {
  height: 85vh;
  box-shadow: 3px 3px 6px rgba(1, 0.2, 1, 0.2);

  width: 90vw;
  margin: 0 auto; /* Centrar horizontalmente */
  margin-top: 10rem; /* Centrar verticalmente */
  border-radius: 10px;
  display: flex;
  justify-content: space-between;
  overflow: hidden;
  
  .left-content {
    /* Fondo gris */
        padding: 4rem;
        text-align: center;
        color: $blanco;
        text-shadow: #000000;
        width: 50%;
        background: linear-gradient($bg, $degradado),
        url("src/assets/buenosaires.jpg") no-repeat center/cover;
        box-sizing: border-box;
        .left-content-inner {
            margin-right: 40px; /* Agregar margen derecho de 40px entre el título y el texto */
            .text-login {
                margin-top: 15rem;
                text-align: center;
                margin: 6rem 0;
                font-size: 2rem;
            }
            .tittle {
                font-size: 4rem;
                font-weight: bolder;
                color: transparent;
                -webkit-text-stroke: 0.2rem $blanco;
            }

            .text1 {
                font-size: 2.5rem;
                margin-top: 25%;
                text-align: center;
                font-weight: bolder;
            }
        }
    }

    .login-container {
        width: 50%;
        height: auto;
        display: flex;
        flex-direction: column;
        align-items: center;
        text-align: center;
        padding-top: 3rem;
        background-color: $secondary;
        h1 {
            margin: 0;
            color: $azul;
            font-weight: bold;
            font-size: 3rem; /* Aumenta el tamaño de la fuente */
            letter-spacing: 1px; /* Aumenta el espaciado entre las letras */
            text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.456); /* Añade una sombra al texto */
            font-family: "Arial", sans-serif; /* Cambia la fuente (opcional) */
        }

        .edit-form{
            .guardar {
                width: 70%;
                padding: 10px;
                margin-top: 45px;
                background-color: $blue;
                color: #fff;
                border: none;
                border-radius: 5px;
                margin-bottom: 2rem;
                cursor: pointer;
            }
            select {
                width: 50%;
                margin-top: 10rem;
                margin: 10px 0;
                padding: 10px;
                border: 1px solid #ccc;
                border-radius: 5px;
            }
            label {
                padding-right:1.1rem;
                font-weight: bolder;
            }

        }
        
    }
}
</style>

<script>
import modifyService from '@/services/shoppingCartServices/modifyItemService.js';
  export default {
    data() {
      return {
            seatQuantity: '',
            seatClass: '',
            flightInfo: {
                flightId: '', // o cualquier otra propiedad que necesites inicializar
                origin: '',
                destination: '',
                flightDate: '',
                costByPerson: '',
                costByPersonOffer: '',
            }
        };
    },
    mounted() {
      this.flightInfo = JSON.parse(sessionStorage.getItem('flightToEdit'));
      if (!this.flightInfo) {
        // Si no hay información de vuelo en sessionStorage, redirige a la página anterior o maneja el caso según tu flujo
        this.$router.push('/'); // Redirige a la página principal o una página de error, según tu flujo
      } else {
        // Puedes establecer valores predeterminados aquí si lo necesitas
        this.seatQuantity = this.flightInfo.seatQuantity ;
        this.seatClass = this.flightInfo.seatClass ;
      }
    },
    methods: {
        async updateCart() {
            // Actualizar la información de asientos y clase en el objeto flightInfo
            this.flightInfo.seatQuantity = this.seatQuantity;
            this.flightInfo.seatClass = this.seatClass;

            try {
                const response = await modifyService.modifyItemInCart(
                    this.flightInfo.userID,
                    this.flightInfo.flightId, // Asegúrate de utilizar flightId en lugar de flightID si ese es el nombre correcto
                    this.flightInfo.seatQuantity,
                    this.flightInfo.seatClass
                );

                console.log("Response from server:", response.data); // Verificar la respuesta del servidor
                const editedFlightIndex = sessionStorage.getItem('flightToEditIndex');
                
                if (editedFlightIndex !== -1) {
                    const editedFlight = { ...this.cartItems[editedFlightIndex] };
                    // Realiza los cambios necesarios en el vuelo
                    editedFlight.seatQuantity = this.flightInfo.seatQuantity;
                    editedFlight.seatClass = this.flightInfo.seatClass;

                    // Actualiza el vuelo en cartItems
                    this.cartItems.splice(editedFlightIndex, 1, editedFlight);
                } else {
                    console.error('Vuelo no encontrado en el carrito.');
                }
                // Eliminar el índice del vuelo editado de sessionStorage
                sessionStorage.removeItem('flightToEditIndex');

                // Redirigir de vuelta a la página del carrito de compras
                this.$router.push('/Carrito');
                // Redirigir solo si la respuesta es exitosa
            
            } catch (error) {
            console.error("Error:", error); // Manejar el error
            // Mostrar un mensaje de error al usuario o realizar alguna acción adecuada
            }
          
        },
        formatDate(dateString) {
            //Cambia el formato de la fecha de milisegundos a años, meses y dias
            const options = { year: "numeric", month: "long", day: "numeric" };
            const date = new Date(dateString);
            return date.toLocaleDateString("es-ES", options);
        },
       
    }
  };
  </script>
  
