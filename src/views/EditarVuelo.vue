
<template>
    <div class="page-container">
      <spinner :showSpinner="showSpinner"></spinner>
      <div class="left-content">
        <hr />
        <h1 class="tittle">Edita los vuelos </h1>
        <hr />
        <div class="text-login">
          <p class="text1">
            ¡Ayuda a tus clientes a escoger un vuelo y comencemos a explorar el
            mundo juntos!
          </p>
        </div>
      </div>
      <div class="creacionV-container">
        <!-- Contenido para crear vuelos -->
        <hr />
        <h1>Editar Vuelo</h1>
        <hr />
        <hr />
        <form @submit.prevent="updateFlightDetails" id="flight-form">
          <label for="costByPerson">Costo por Persona</label>
            <input type="number" id="costByPerson" placeholder="Costo por persona" v-model="flightDetails.costByPerson" required />
        

          <label for="costByPerson">Costo de Oferta por persona</label>
            <input type="number" id="costByPersonOffer" placeholder="Costo de oferta por persona" v-model="flightDetails.costByPersonOffer" required />
        

          <label for="costByPerson">Sillas Disponibles</label>
            <input type="number" id="availableSeats" placeholder="Sillas disponibles" v-model="flightDetails.availableSeats" required />
    
          <button id="create-flight" class="login" type="submit">
            Guardar 
          </button>
        </form>
        <!-- Mostrar mensaje de error como modal si es necesario -->
        
        <success-modal :show-note="showSuccessMessage" :success-message="successMessage"
            @close="showSuccessMessage = false" />
      </div>
    </div>
    <!-- Footer aquí -->
    <Footer></Footer>
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
    height: 50%;
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
        url("src/assets/madrid.jpg") no-repeat center/cover;
      box-sizing: border-box;
  
      .text-login {
        margin-top: 15rem;
        text-align: center;
        margin: 6rem 0;
        font-size: 2rem;
      }
      .tittle {
        font-size: 3rem;
        font-weight: bolder;
        color: transparent;
        -webkit-text-stroke: 0.2rem $blanco;
      }
  
      .text1 {
        font-size: 2rem;
        margin-top: 25%;
        text-align: center;
        font-weight: bolder;
        margin: 8rem 0;
      }
    }
  
    .creacionV-container {
      width: 50%;
      display: flex;
      flex-direction: column;
      align-items: center;
      text-align: center;
      padding-top: 2rem;
      padding-bottom: 9rem;
      background-color: $secondary;
  
      h1 {
        color: $azul;
        font-weight: bold;
        font-size: 3rem; /* Aumenta el tamaño de la fuente */
        letter-spacing: 1px; /* Aumenta el espaciado entre las letras */
        text-shadow: 2px 2px 2px rgba(0, 0, 0, 0.456); /* Añade una sombra al texto */
        font-family: "Arial", sans-serif; /* Cambia la fuente (opcional) */
      }
  
      input {
        width: 100%;
        margin-top: 10rem;
        margin: 10px 0;
        padding: 10px;
        border: 1px solid #ccc;
        border-radius: 5px;
        display: flex;
      }
  
      .login {
        width: 80%;
        padding: 10px;
        margin-top: 3rem;
        background-color: $blue;
        margin-bottom: 1rem;
        color: #fff;
        border: none;
        border-radius: 5px;
        cursor: pointer;
      }
  
      .tittle {
        margin-top: 20px;
        font-size: 5rem;
      }
  
      .text2 {
        font-family: "Courier New", Courier, monospace;
        //font-size: small;
        text-align: center;
        margin-top: 10px;
        font-weight: bolder;
      }
  
      .error-message {
        color: red;
      }
    }
    label {
      display: flex;
      margin-bottom: 5px;
      font-weight: bolder;
      font-size: 1.6rem;
      color: $negro;
      margin-top: 2rem;
    
    }
    @media screen and (max-width: 1000px) {
      .page-container {
        width: 70%;
        margin-top: 10rem;
      }
      .creacionV-container {
        width: 100%;
        border-radius: 5px;
      }
  
      .left-content {
        display: none;
      }
    }
  
    @media screen and (max-width: 650px) {
      .page-container {
        width: 90%;
      }
    }
  
    @media screen and (max-width: 500px) {
      .page-container {
        height: 100%;
        margin-top: 10rem;
      }
  
      .creacionV-container {
        .texto {
          margin-top: 5rem;
        }
      }
    }
  }
  </style>
  
  
  
  <script>
  import errorModal from "@/components/errorModal.vue";
  import spinner from "@/components/spinner.vue";
  import Footer from "@/components/footer.vue";
  import flightService from "@/services/FlightService/editFlightService.js"; // Replace with the actual path to your service
  import flightServiceDetail from "@/services/FlightService/detailFlightService.js";
  import successModal from "@/components/successModal.vue";
  export default {
    data() {
        return {
        showSpinner: false, // Initialize as hidden
        flightId: null,
        successMessage: null,
        showSuccessMessage: false,
        flightDetails: {
            image: null,
            costByPerson: null,
            costByPersonOffer: null,
            availableSeats: null,
            seats: null,
            // other flight details...
        },
        // other data properties...
        };
    },
    
    async mounted() {
        // Get the flight ID from the session storage
        this.flightId = window.sessionStorage.getItem('editFlightId');
        console.log("vuelo a editar", this.flightId );
        try {
            const response = await flightServiceDetail.getDetailFlight(this.flightId);
            console.log("vuelo datos: ", response.data );
            this.flightDetails = response.data;
            } catch (error) {
            console.error("Error al obtener los detalles del vuelo:", error);
         }
        // other code...
    },
    
    methods: {
        async updateFlightDetails() {
            try {
                const response = await flightService.editFlight(
                    this.flightId,
                    this.flightDetails.image,
                    this.flightDetails.costByPerson,
                    this.flightDetails.costByPersonOffer,
                    this.flightDetails.availableSeats,
                    this.flightDetails.seats,
                   
                );
                this.successMessage = "¡Datos actualizados correctamente!";
                this.showSuccessMessage = true;
                this.showSpinner = false;
            } catch (error) {
                console.error("Error updating flight details:", error);
                this.errorMessage = "Ha ocurrido un error al editar el vuelo.";
            }
        },
    },
        components: {
            errorModal,
            spinner,
            Footer,
            successModal,
        },
   };
  </script>