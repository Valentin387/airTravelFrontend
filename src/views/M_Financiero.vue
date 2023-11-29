<template>
    <div class="payment-module">
      <div class="menu">
        <nav>
          <ul>
            <li><a @click="showAddCardForm">Agregar tarjeta</a></li>
            <li><a @click="showUserCards">Ver tarjetas</a></li>
          </ul>
        </nav>
      </div>
    <br>
    <div class="container-agregar" v-if="showAddCard"> 
      <h1 class="tittle">Agregar tarjeta</h1>
      <div class="payment-options">
        <p class="payment-label"><strong>Pagar con:</strong> </p>
        <label @click="selectOption('credito')" :class="{ selected: selectedOption === 'credito' }">Tarjeta de crédito</label>
        <label @click="selectOption('debito')" :class="{ selected: selectedOption === 'debito' }">Tarjeta de débito</label>
      </div>
      <div v-if="selectedOption === 'credito'">
        <h2>Tarjeta de crédito</h2>
        <div class="paypal-details">
          <input type="text" placeholder="Nombre de titular" v-model="cardholderName" @input="restrictToLetters">
          <input type="number" placeholder="Número de tarjeta" v-model="creditCardNumber" @input="restrictToNumbers">
          <div class="expiration-cvc">
            <select v-model="expirationMonth">
              <option disabled value="">MM</option>
              <option v-for="month in 12" :key="month" :value="month">
                {{ month < 10 ? '0' + month : month }}
              </option>
            </select>
            <input type="number" placeholder="YY" v-model="expirationYear" @input="restrictYear">
            <input type="number" placeholder="CVC" v-model="cvc" @input="restrictCVC">
            <input type="number" placeholder="Saldo" v-model="balance" @input="restrictBalance">

          </div>
          <button class="button-confi" @click="addNewCard">Guardar tarjeta</button>
        </div>
      </div>
      <div v-if="selectedOption === 'debito'">
        <h2>Tarjeta de débito</h2>
        <div class="paypal-details">
          <input type="text" placeholder="Nombre de titular" v-model="cardholderName" @input="restrictToLetters">
          <input type="number" placeholder="Número de tarjeta" v-model="creditCardNumber" @input="restrictToNumbers">
          <div class="expiration-cvc">
            <select  v-model="expirationMonth">
              <option disabled value="">MM</option>
              <option v-for="month in 12" :key="month" :value="month">
                {{ month < 10 ? '0' + month : month }}
              </option>
            </select>
            <input type="number" placeholder="YY" v-model="expirationYear" @input="restrictYear">
            <input type="number" placeholder="CVC" v-model="cvc" @input="restrictCVC">
            <input type="number" placeholder="Saldo" v-model="balance" @input="restrictBalance">

          </div>
          <button class="button-confi" @click="addNewCard">Guardar tarjeta</button>
         
        </div>
      </div>
   
    </div>
    <div  class="container" v-if="showUserListCard">
      <h1>Tarjetas del Usuario</h1>
      <div class="container-Lista">
        <div v-for="(card, index) in userCards" :key="index" class="card">
          <button class="button-delete" @click="removeCard(card.id)">X</button>
          <div>
            <strong>Tipo:</strong> {{ card.type }} 
          </div>
          <div>
            <strong>Número:</strong> {{ card.number }}
          </div>
          <div>
            <strong>Nombre del Titular:</strong> {{ card.name }}
          </div>
          <!-- Agrega más detalles según la estructura de tus tarjetas -->
          <hr />
          <div>
            <strong>Balance:</strong> {{ card.balance }}
          </div>
        </div>
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
$verde: rgb(0, 189, 142);
$blanco: #ffffff;
$negro: #1a1320;
$accent: #0b97f4;
$accent3: #77797a;
$blue: #54b2f1;
$secondary: #ceeafd;
$azul: #0d629b;
$card: #0d629b17;
$bodycol:#e6eff6;

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


  .payment-module {
    margin: 0 auto;
    padding: 20px;
    border-radius: 5px;
    height: auto;
    width: 90vw;
    margin: 0 auto; /* Centrar horizontalmente */
    margin-top: 10rem; /* Centrar verticalmente */
    background-color: $secondary;
    .menu {
      display: flex;
      justify-content: space-around;
      list-style-type: none;
      margin: 0;
      padding: 0;
      overflow: hidden;
      background-color: $azul;
    }

    .menu li {
      float: left;
      text-decoration: none;
      list-style: none; /* Agrega esta línea */
    }

    .menu li a {
      display: block;
      color: white;
      text-align: center;
      padding: 14px 16px;
      text-decoration: none;
    }

    .menu li a:hover {
      background-color: $blue;
    }

    .payment-options {
        display: flex;
        justify-content: space-between;
        align-items: center; /* Alinear verticalmente los elementos al centro */
        margin-top: 10px;

        .payment-label {
        text-align: center; /* Alinear el texto al centro horizontalmente */
        }

        label {
        cursor: pointer;
        padding: 10px;
        margin: 10px 70px 30px 70px;
        text-align: center; /* Alinear texto al centro */
        font-weight: bolder;
        &.selected {
            color: #4b7ce7; /* Cambiar el color de texto cuando está seleccionado */
        }
        }
    }

    .tittle{
        text-align: center;
        font-size: 30px;
    }
  
    .paypal-details {
      margin-top: 20px;
  
      input {
        /*border: 1px solid #0070ba; 
        padding: 10px;
        margin-top: 5px;
        width: 100%;*/
        width: 100%;
        margin-top: 10rem;
        margin: 10px 0;
        padding: 10px;
        border: 1px solid #ccc;
        border-radius: 5px;
      }
      select{
        width: 30%;
        margin-top: 10rem;
        margin: 10px 0;
        padding: 10px;
        border: 1px solid #ccc;
        border-radius: 5px;
        width: calc(50% - 1px); /* Dividir en dos columnas con espacio entre ellos */
        margin-right: 10px;
      }
  
      .expiration-cvc {
        display: flex;
        justify-content: space-between;
        input[type="number"] {
          /*border: 1px solid #0070ba;
          padding: 10px;*/
          margin-top: 10rem;
          margin: 10px 0;
          padding: 10px;
          border: 1px solid #ccc;
          border-radius: 5px;
          width: calc(50% - 1px); /* Dividir en dos columnas con espacio entre ellos */
          & + input[type="number"] {
            margin-left: 10px;
          }
        }
      }

      .checkbox-label {
        display: flex;
        align-items: center;

            input[type="checkbox"] {
                
                width: 20px;
                height: 20px;
                border: 1px solid #0070ba; /* Estilo del cuadro de selección */
                border-radius: 3px;
                margin-right: 10px; /* Espacio entre el cuadro de selección y el texto */
                cursor: pointer;
            }

            input[type="checkbox"]:checked {
                background-color: #0070ba; /* Cambiar el fondo cuando está seleccionado */
                border: 1px solid #0070ba; /* Cambiar el borde cuando está seleccionado */
            }
        }
    }

    .button-confi {
        margin-top: 40px; /* Margen superior de 40px */
        border-radius: 5px; /* Borde redondeado */
        background-color: #0070ba; /* Color de fondo azul */
        color: #fff; /* Color del texto blanco */
        padding: 10px 20px;
        border: none;
        cursor: pointer;
        transition: background-color 0.3s; /* Efecto de transición al cambiar el color de fondo */
    }

    .button-confi:hover {
        background-color: #00558a; /* Cambiar el color de fondo al pasar el mouse */
    }
  }
  .container-Lista {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
  
  }
  .container-Lista .button-delete {
    float: right;
    background-color: rgb(212, 8, 8);
    color: white;
    border-radius: 15px;
    width: 20px;
    text-align: center;
    cursor: pointer;
    
  }

  .card {
    width: 300px;
    padding: 20px;
    margin: 10px;
    background:#54b2f138;
    border: 1px solid $azul;
    box-shadow: 0px 5px 6px rgba(5, 0, 0, 0.2);
    border-radius: 5px;
    
  }
</style>
  
<script>
import FinancialService from "@/services/finantialModuleService/addCardService.js";
import FinancialServiceList from "@/services/finantialModuleService/listCardsService.js";
import FinancialServiceRemove from "@/services/finantialModuleService/removeCardService.js";
import FinancialServiceEdit from "@/services/finantialModuleService/editBalanceService.js";
import errorModal from "@/components/errorModal.vue";
import spinner from "@/components/spinner.vue";
import Footer from "@/components/footer.vue";

  export default {
    data() {
      return {
        token: window.sessionStorage.getItem("JWTtoken"),
        userCards: [], // Arreglo para almacenar las tarjetas del usuario
        userID: '',
        balance: '',
        selectedOption: 'credito',
        saveCardCheckbox: false, // Variable para controlar si se debe guardar la tarjeta
        expirationMonth: '',
        expirationYear: '',
        creditCardNumber: '',
        cardholderName: '',
        cvc: '',
        titularName: '',
        showAddCard: true, // Variable para controlar la visualización del formulario
        showUserListCard: false, // Variable para controlar la visualización de las tarjetas del usuario
      };
    },
    mounted() {
      this.getUserCards();
    },
    methods: {
      showAddCardForm() {
        this.showAddCard = true;
        this.showUserListCard = false; // Variable para controlar la visualización de las tarjetas del usuario
      },

      // Método para ocultar el formulario de agregar tarjeta
      showUserCards() {
        this.showAddCard = false;
  
        this.showUserListCard = true;
        // Agrega lógica adicional si necesitas manejar la vista de las tarjetas del usuario
      },
      async removeCard(cardId) {
        try {
          await FinancialServiceRemove.removeCard(cardId);
          // Vuelve a cargar las tarjetas después de eliminar una
          this.getUserCards();
          console.log('Tarjeta eliminada con éxito');
          window.alert('Tarjeta eliminada con éxito');
        } catch (error) {
          console.error('Error al eliminar la tarjeta:', error.message);
          // Manejo de errores
        }
      },
      async getUserCards() {
        try {
          const token = window.sessionStorage.getItem("JWTtoken");//Obtener el token 
            if (token && token != null) {
                const token = window.sessionStorage.getItem("JWTtoken");
                if (this.token) {
                  const tokenData = JSON.parse(atob(token.split('.')[1]));
                  this.userID = tokenData.ID;
                  console.log('usuario:', this.userID);  //Obtener el ID de usuario del token
                }
            }
          // Llama al servicio listCards con el ID del usuario
          const response = await FinancialServiceList.listCards(this.userID);

          // Actualiza el arreglo userCards con las tarjetas obtenidas
          this.userCards = response.data; // Suponiendo que el servicio devuelve un arreglo de tarjetas
        } catch (error) {
          console.error('Error al obtener las tarjetas:', error.message);
          // Manejo de errores
        }
      },
      addNewCard() {
        const token = window.sessionStorage.getItem("JWTtoken");//Obtener el token 
            if (token && token != null) {
                const token = window.sessionStorage.getItem("JWTtoken");
                if (this.token) {
                  const tokenData = JSON.parse(atob(token.split('.')[1]));
                  this.userID = tokenData.ID;
                  console.log('usuario:', this.userID);  //Obtener el ID de usuario del token
                }
            }
        const cardData = {
          
          userID: this.userID, // Reemplaza con el ID del usuario
          balance: this.balance, // Define el saldo inicial
          type: this.selectedOption === 'credito' ? 'credito' : 'debito', // Verifica el tipo de tarjeta seleccionado
          number: this.creditCardNumber, // Número de tarjeta ingresado en el campo
          name: this.cardholderName, // Nombre del titular ingresado
          expirationDate: `${this.expirationMonth}/${this.expirationYear}`, // Fecha de expiración
          cvc: this.cvc // Código de seguridad CVC
        };
   

        FinancialService.addCard(cardData.userID, cardData.balance, cardData.type, cardData.number, cardData.name, cardData.expirationDate, cardData.cvc)
          .then(response => {
            console.log('Tarjeta agregada:', response);
            // Lógica adicional después de agregar la tarjeta
          })
          .catch(error => {
            console.error('Error al agregar la tarjeta:', error.message);
            // Manejo de errores
          });
      },
      restrictBalance() {
        const maxLength = 10; // Por ejemplo, máximo de 10 dígitos permitidos para el saldo
        const enteredBalance = String(this.balance);

        // Verificar si el saldo ingresado contiene caracteres no numéricos

        // Limitar la longitud del saldo a maxLength
        if (enteredBalance.length > maxLength) {
          
        }
      },

      // Método para restringir el año a 2 dígitos
      restrictYear() {
          if (String(this.expirationYear).length > 2) {
            this.expirationYear = Number(String(this.expirationYear).slice(0, 2));
        }
      },
      restrictToNumbers() {
        if (String(this.creditCardNumber).length > 19) {
          this.creditCardNumber = Number(String(this.creditCardNumber).slice(0, 19));
        }
      },
      restrictToLetters() {
        this.cardholderName = this.cardholderName.replace(/[^A-Za-z\s]/g, '');
      },
      restrictCVC() {
     
        if (String(this.cvc).length > 3) {
          this.cvc = Number(String(this.cvc).slice(0, 3));
        }
      },
      validateTitularName() {
        // Verificar si el nombre del titular solo contiene espacios en blanco
        if (/^\s+$/.test(this.titularName)) {
          // Si solo contiene espacios, borra el contenido del campo
          this.titularName = '';
        }
      },
      selectOption(option) {
        this.selectedOption = option;
      },
      saveCard() {
        if (this.saveCardCheckbox) {
          // Lógica para guardar la tarjeta de Paypal
          alert('Tarjeta guardada');
        }
      },
      confirmPayment() {
        // Lógica para confirmar el pago
        alert('Pago confirmado');
      },
    },
    components: {
    errorModal,
    spinner,
    Footer,
  },
  };
</script>
  