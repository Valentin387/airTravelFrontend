<template>
  <div class="list_container">
    <div class="buttons-container">
      <div class="left-buttons">
        <button class = "activos" @click="showFlights('ON_TIME')" :class="{ activeButton: filter === 'ON_TIME' }">Activos</button>
        <button class= "realizados" @click="showFlights('DEPARTED')" :class="{ activeButton: filter === 'DEPARTED' }">Realizados</button>
        <button class = "cancelados" @click="showFlights('CANCELLED')" :class="{ activeButton: filter === 'CANCELLED' }">Cancelados</button> 
       
      </div>
      <p> <strong> Lista de Vuelos</strong> </p>
      
    </div>
    <div class="flight-list">
      <div class="tittle-container">
        <table>
          <thead>
            <tr> 
              <th class="left-align">Vuelo</th>
              <th class="left-align">Fecha de Salida</th>
              <th class="left-align">Duración</th>
              <th class="left-align">Fecha de Llegada</th>
              <th class="left-align">Costo por Persona</th>
              <th class="left-align">Oferta</th>
              <th class="left-align">Asientos Disponibles</th>
              
            </tr>
          </thead>
          <tbody>
            <tr v-if="filteredFlights.length === 0">
              <td class="warning" colspan="3">No se encontraron vuelos</td>
            </tr>
            <tr v-for="flight in filteredFlights" :key="flight.id" class="flight-item">
              <td class="flight">{{ flight.origin }} - {{ flight.destination }} </td>
              <td>{{ formatDate(flight.flightDate) }}</td>
              <td> {{ formatDuration(flight.flightDuration) }} 
                <span class="letrahoras" v-if="isDurationHours(flight.flightDuration)" > horas </span>
                <span v-else>minutos</span></td>
              <td> {{ formatDate(flight.arrivalDate) }}</td>
              <td>
                <template v-if="flight.costByPersonOffer !== null && flight.costByPersonOffer > 0">
                  <del>{{ flight.costByPerson }}</del>
                </template>
                <template v-else>
                  {{ flight.costByPerson}}
                </template>
              </td>
              <td>{{ flight.costByPersonOffer	}}</td>
              <td>{{ flight.availableSeats	}}</td>
              
              <td>
                <!-- Add the click event to the delete button -->
                <!-- Add the click event to the cancel button -->
                <button class="button-delete" @click="logIdAndCancelFlight(flight.id)">x</button>
              </td>
              <td><button class="editar" @click="EditFlight(flight.id)">Editar</button> </td>
            </tr>
          </tbody>
        </table>
        
      </div>
      <div class="right-button">
          <button class="crear" href= "/CrearVuelo" @click="createFlight">Crear Vuelo</button>
         
        </div>
    </div>
  </div>
  <!------------------------------------------------FOOTER------------------------------------------->
  <Footer></Footer>
</template>

<style lang="scss">
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

td del {
  text-decoration: line-through;
}

  .list_container {
    margin: 0 auto;
    padding: 20px;
    border-radius: 20px;
    height: 100vh;
    width: 90vw;
    margin: 0 auto; /* Centrar horizontalmente */
    background: $secondary;
    margin-top: 10rem; /* Centrar verticalmente */

    .buttons-container {
      display: flex;
      justify-content: space-between;
  
      p{
        font-size: 2.5rem;
        padding: 1rem 2rem;
        text-align: center;
        margin: 0 auto;
      }

        .left-buttons {
          display: flex;
          gap: 10px; /* Espacio entre los tres primeros botones */
          padding: 1rem 2rem;
          font-size: 1.7rem;
      
      }
      .right-button {
        text-align: center;
        background-color: transparent;
        color: white;
        .crear{
          margin: 41rem;
        }
       
        
      
      }
   }

   
  }



  
  .activos:hover {
    background-color: #54b2f1; /* Cambia el color de fondo al pasar el mouse sobre los botones "Activos", "Realizados" y "Cancelados" */
    border: 0.5rem solid #0f293a17;
    color: white;
    margin: 1rem auto; //Centrado 
    display: inline-block;
  }
  .cancelados:hover {
    background-color: #54b2f1; /* Cambia el color de fondo al pasar el mouse sobre los botones "Activos", "Realizados" y "Cancelados" */
    border: 0.5rem solid #0f293a17;
    color: white;
    margin: 1rem auto; //Centrado 
    display: inline-block;
   
  }
  .realizados:hover {
    background-color: #54b2f1; /* Cambia el color de fondo al pasar el mouse sobre los botones "Activos", "Realizados" y "Cancelados" */
    border: 0.5rem solid #0f293a17;
    color: white;
    margin: 1rem auto; //Centrado 
    display: inline-block;
  }


  .tittle-container {
    border: 1px solid #0d629b17;
    background:  #f2f2f283;
    border-radius: 5px;
    max-height: 300px; 
    overflow-y: auto;
    margin-top: 20px;
    


  }

  table {
    width: 100%;
    border-collapse: collapse;
  }

 

  .flight{
    text-align: left;
  }

  .warning{
    font-size: 20px;
    display: center;
    padding-top: 5%;
  }

 

  .flight-item {
    border: 2px solid #0d629b17;; /* Agrega un borde de color gris oscuro a cada vuelo */
    th, td {
      padding: 8px;
      text-align: center; 
    }
    .editar{
          background-color: transparent;
          font-weight: bolder;
          color:$azul;
      }
    .button-delete {
      background-color:  #ea1e1e;
      color: white;
      border-radius: 15px;
      cursor: pointer;
    }

    .button-delete:hover {
      background-color: #54b2f1; /* Cambia el color de fondo al pasar el mouse sobre el botón "Eliminar" */
    }
  }
  
  .left-buttons button {
    background:  #f2f2f283;
    color: $azul;
    padding-inline: 1rem;//Espacio entre texto y borde del recuadro
    border: 3px solid #0d629b17;
    border-radius: 5px;
  }

  .activeButton {
    background-color: #54b2f1;
    color: white;
    border: 10px solid #54b2f1;
    border-radius: 5px;
  }

  .right-button button {
    background-color: $gris2;
    color: $blanco;
    margin: 1rem auto; //Centrado 
    display: inline-block;
    padding: 1rem 2rem;
    font-size: 1.7rem;
    border: $azul-claro .2rem solid;
    border-radius: 5rem;
    box-shadow: inset 0px 0px 0px 1px $negro;
  }
</style>

<script>
import errorModal from "@/components/errorModal.vue";
import spinner from "@/components/spinner.vue";
import Footer from "@/components/footer.vue";
import flightService from "@/services/FlightService/listByStateService.js";
import cancelFlightService from "@/services/FlightService/deleteFlightService.js";
import successModal from "@/components/successModal.vue";
export default {
  data() {
    return {
      showSpinner: false, // Initialize as hidden
      successMessage: null,
      showSuccessMessage: false,
      flights: [],
      filter: 'ON_TIME',
    };
  },
  created() {
    this.fetchFlightsByState('ON_TIME'); // Obtener vuelos activos al cargar la página
  },
  computed: {
    filteredFlights() {
      return this.flights.filter(flight => {
        if (this.filter === 'ON_TIME') {
          return flight.state === 'ON_TIME';
        } else if (this.filter === 'DEPARTED') {
          return flight.state === 'DEPARTED';
        } else if (this.filter === 'CANCELLED') {
          return flight.state === 'CANCELLED';
        }
        return false;
      });
    },
  },
  methods: {
    async fetchFlightsByState(state) {
      try {
        const response = await flightService.getFlightsByState(state);
        this.flights = response.data;
      } catch (error) {
        console.error("Error al obtener vuelos:", error);
      }
    },
    formatDate(dateString) {
     //Cambia el formato de la fecha de milisegundos a años, meses y dias
     const options = { year: "numeric", month: "long", day: "numeric" };
     const date = new Date(dateString);
     return date.toLocaleDateString("es-ES", options);
   },
   formatDuration(duration) {
     //formato simplificado de tiempo de duración del vuelo ( reemplaza 00:00:00)
     const parts = duration.split(":").map((part) => parseInt(part, 10));
     const hours = parts[0];
     const minutes = parts[1];

     if (hours > 0) {
       return hours;
     } else {
       return hours * 60 + minutes;
     }
   },

   isDurationHours(duration) {
     //Permite saber si la duración del vuelo será en horas o minutos.( reemplaza 00:00:00)
     const parts = duration.split(":").map((part) => parseInt(part, 10));
     const hours = parts[0];
     return hours > 0;
   },
    showFlights(filter) {
      this.filter = filter;
      this.fetchFlightsByState(filter);
    },
    createFlight() {
      this.$router.push("/CrearVuelo");
    },

    logIdAndCancelFlight(id) {
    console.log("vuelo a cancelar", id);
    this.cancelFlight(id);
  },

    async cancelFlight(id) {
      if (window.confirm('¿Estás seguro de que quieres borrar este vuelo?')) {
        try {
          await cancelFlightService.deleteFlight(id);
          this.successMessage = "¡Vuelo Cancelado correctamente!";
          this.showSuccessMessage = true;
          this.showSpinner = false;
          // Refresh the flights after cancellation
          this.fetchFlightsByState(this.filter);
        } 
        catch (error) {
          console.error("Error cancelling flight:", error);
        }
      }
    },
    EditFlight(flightId) {
      // Store the flight ID in the session storage
      window.sessionStorage.setItem('editFlightId', flightId);
      // Navigate to the EditarVuelo view
      this.$router.push('/EditarVuelo');
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
