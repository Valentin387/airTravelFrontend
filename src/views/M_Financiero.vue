<template>
    <div class="payment-module">
      <spinner :showSpinner="showSpinner"></spinner>
      <div class="menu">
        <nav>
          <ul>
            <li><a @click="showAddCardForm"><span class="material-symbols-outlined">add_card</span> Agregar tarjeta</a></li>
            <li><a @click="showUserCards"><span class="material-symbols-outlined">credit_card</span> Ver tarjetas</a></li>
          </ul>
        </nav>
      </div>
    <br>
    <div class="container-agregar" v-if="showAddCard"> 
      <h1 class="tittle">Agregar Tarjeta</h1>
      <div class="payment-options">
        <p class="payment-label"><strong >Pagar con:</strong> </p>
        <label @click="selectOption('credito')" :class="{ selected: selectedOption === 'credito' }">Tarjeta de crédito</label>
        <label @click="selectOption('debito')" :class="{ selected: selectedOption === 'debito' }">Tarjeta de débito</label>
      </div>
      <div v-if="selectedOption === 'credito'">
        <h2>Tarjeta de crédito</h2>
        <div class="paypal-details">
          <input type="text" placeholder="Nombre de titular" v-model="cardholderName" >
          <input type="number" placeholder="Número de tarjeta" v-model="creditCardNumber" >
          <div class="expiration-cvc">
            <select v-model="expirationMonth">
              <option disabled value="">MM</option>
              <option v-for="month in 12" :key="month" :value="month">
                {{ month < 10 ? '0' + month : month }}
              </option>
            </select>
            <input type="number" placeholder="YY" v-model="expirationYear" >
            <input type="number" placeholder="CVC" v-model="cvc" >
            <input type="number" placeholder="Saldo" v-model="balance" >

          </div>
          <button class="button-confi" @click="validateAndAddCard">Guardar tarjeta</button>
        </div>
      </div>
      <div v-if="selectedOption === 'debito'">
        <h2>Tarjeta de débito</h2>
        <div class="paypal-details">
          <input type="text" placeholder="Nombre de titular" v-model="cardholderName" >
          <input type="number" placeholder="Número de tarjeta" v-model="creditCardNumber" >
          <div class="expiration-cvc">
            <select  v-model="expirationMonth">
              <option disabled value="">MM</option>
              <option v-for="month in 12" :key="month" :value="month">
                {{ month < 10 ? '0' + month : month }}
              </option>
            </select>
            <input type="number" placeholder="YY" v-model="expirationYear" >
            <input type="number" placeholder="CVC" v-model="cvc">
            <input type="number" placeholder="Saldo" v-model="balance">
          
          </div>
          <button class="button-confi" @click="validateAndAddCard">Guardar tarjeta</button>
         
        </div>
      </div>
   
    </div>
    <div  class="container" v-if="showUserListCard">
      <h1 class="tittle"> Mis Tarjetas </h1>
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
          <hr />
          <div>
            <strong>Balance:</strong> {{ card.balance }}
          </div>
          <button class="button-edit" @click="editCardBalance(card.id)">Editar</button>
        </div>
      </div>
    </div>
      
  </div>
  <success-modal :show-note="showSuccessMessage" :success-message="successMessage" @close="showSuccessMessage = false" />
  <error-modal :show-error="showErrorMessage" :error-message="errorMessage" @close="showErrorMessage = false" />
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
$verde:  #4caf50;
$blanco: #ffffff;
$negro: #1a1320;
$accent: #0b97f4;
$accent3: #77797a;
$blue: #54b2f1;
$secondary: #ceeafd;
$azul: #0d629b;
$card: #0d629b17;
$bodycol:#e6eff6;
$boton: #0070ba;
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

.material-symbols-outlined {
  position: relative;
  top: 5px;
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
       // justify-content: space-between;
        align-items: center; /* Alinear verticalmente los elementos al centro */
        margin-top: 10px;
        font-size: 2rem;
        padding-top: 2rem;

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
            color: $verde; /* Cambiar el color de texto cuando está seleccionado */
        }
        }
    }

    .tittle{
        text-align: center;
        font-size: 30px;
        margin: 0;
        color: $azul;
        padding-top:2rem ;
        padding-bottom: 2rem;
        font-weight: bold;
        font-size: 3rem; /* Aumenta el tamaño de la fuente */
        letter-spacing: 1px; /* Aumenta el espaciado entre las letras */
        text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.456); /* Añade una sombra al texto */
        font-family: "Arial", sans-serif; /* Cambia la fuente (opcional) */
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

  .button-edit {
    background-color: $verde; /* Color verde */
    color: white;
    border-radius: 5px;
    padding: 8px 16px;
    margin-left: auto;
    cursor: pointer;
    transition: background-color 0.3s;

    &:hover {
      background-color: #45a049; /* Cambia el color al pasar el mouse */
    }
  }

}

.card {
  width: 300px;
  padding: 20px;
  margin: 10px;
  background: #54b2f138;
  border: 1px solid $azul;
  box-shadow: 0px 5px 6px rgba(5, 0, 0, 0.2);
  border-radius: 5px;
  display: flex;
  justify-content: space-between;
  

  .card-details {
    flex: 1;
    align-items: left;
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
import successModal from "@/components/successModal.vue";

  export default {
    data() {
      return {
        showSpinner: false, // Initialize as hidden
        token: window.sessionStorage.getItem("JWTtoken"),
        successMessage: "",
        showSuccessMessage: false,
        errorMessage: "",
        showErrorMessage: false,
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
        this.showSpinner = true;
       
        // Agrega lógica adicional si necesitas manejar la vista de las tarjetas del usuario
      },
      async removeCard(cardId) {
        // Muestra un mensaje de confirmación antes de eliminar la tarjeta
        if (window.confirm('¿Estás seguro de que quieres eliminar esta tarjeta?')) {
          try {
            await FinancialServiceRemove.removeCard(cardId);
            
            // Vuelve a cargar las tarjetas después de eliminar una
            this.getUserCards();
            console.log('Tarjeta eliminada con éxito');
            this.successMessage = "¡Tarjeta eliminada con éxito!";
            this.showSuccessMessage = true;
            this.showSpinner = false;
          } catch (error) {
            console.error('Error al eliminar la tarjeta:', error.message);
            // Manejo de errores
          }
        }
      },
      async getUserCards() {
        showSpinner = true; // Initialize as hidden
        try {
          
          const token = window.sessionStorage.getItem("JWTtoken");//Obtener el token 
            if (token && token != null) {
                const token = window.sessionStorage.getItem("JWTtoken");
                if (this.token) {
                  const tokenData = JSON.parse(atob(token.split('.')[1]));
                  this.userID = tokenData.ID;
                  console.log('usuario:', this.userID);  //Obtener el ID de usuario del token
                  this.showSpinner = false; // Initialize as hidden
                }
            }
          // Llama al servicio listCards con el ID del usuario
          const response = await FinancialServiceList.listCards(this.userID);

          // Actualiza el arreglo userCards con las tarjetas obtenidas
          this.userCards = response.data; // Suponiendo que el servicio devuelve un arreglo de tarjetas
        } catch (error) {
          console.error('Error al obtener las tarjetas:', error.message);
          this.errorMessage = "Error al obtener las tarjetas";
          this.showErrorMessage = true;
          // Manejo de errores
        }
      },
      validateAndAddCard() {
        // Validar los campos antes de agregar la tarjeta
        if (!this.cardholderName || !this.creditCardNumber || !this.expirationMonth || !this.expirationYear || !this.cvc || !this.balance) {
          this.errorMessage = "Por favor, complete todos los campos.";
          this.showErrorMessage = true;
          return;
        }

        // Realizar validaciones específicas según tus requisitos
        if (this.creditCardNumber.toString().length !== 16  || isNaN(this.creditCardNumber)) {
          this.errorMessage = "El número de tarjeta de crédito debe contener 16 dígitos y ser numérico.";
          this.showErrorMessage = true;
          return;
        }

        if (this.cvc.toString().length !== 3 || isNaN(this.cvc)) {
          this.errorMessage = "El código CVC debe contener 3 dígitos y ser numérico.";
          this.showErrorMessage = true;
          return;
        }
        const currentYear = new Date().getFullYear(); // Obtiene el año actual
        const enteredYear = parseInt(this.expirationYear) ;

        let currentYearLastTwoDigits = parseInt(currentYear.toString().slice(-2));

        if (
          enteredYear.toString().length !== 2 ||
          enteredYear < currentYearLastTwoDigits ||
          enteredYear > currentYearLastTwoDigits + 6 // Rango de fecha de expiración de tarjeta 2023+ 6. Se puede ajustar el rango si es necesario
        ) {
          console.log('current:', currentYearLastTwoDigits); 
          console.log('ingresado año:', enteredYear); 
          this.errorMessage = "Ingrese un año de expiración válido y en el futuro.";
          this.showErrorMessage = true;
          this.expirationYear = ''; // Borra el campo si el año no es válido
          return;
        }
        if (!/^[a-zA-Z\s]*$/.test(this.cardholderName)) {
          this.errorMessage = "El nombre del titular de la tarjeta solo debe contener letras.";
          this.showErrorMessage = true;
          return;
        }
        if (this.cardholderName.trim() === '') {
          this.errorMessage = "El nombre del titular de la tarjeta no puede ser solo espacios en blanco.";
          this.showErrorMessage = true;
          return;
        }
        // Puedes agregar más validaciones según tus requisitos

        // Si todas las validaciones pasan, llama al método addNewCard
        this.addNewCard();
      },
   
      addNewCard() {
        this.showSpinner = true; // Initialize as hidden
        const token = window.sessionStorage.getItem("JWTtoken");//Obtener el token 
            if (token && token != null) {
                const token = window.sessionStorage.getItem("JWTtoken");
                if (this.token) {
                  const tokenData = JSON.parse(atob(token.split('.')[1]));
                  this.userID = tokenData.ID;
                  console.log('usuario:', this.userID);  //Obtener el ID de usuario del token
                  this.showSpinner = false; // Initialize as hidden
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
            this.successMessage = "¡Tarjeta agregada con éxito!";
            this.showSuccessMessage = true;
            this.showSpinner = false;
            window.location.reload();

          })
          .catch(error => {
            console.error('Error al agregar la tarjeta:', error.message);
            this.showSpinner = false;
            // Manejo de errores
          });
      },
      async editCardBalance(cardId) {
        this.showSpinner = true; // Initialize as hidden
        try {
          // Lógica para obtener el nuevo saldo (puedes pedirlo al usuario o proporcionar un campo de entrada)
          const newBalance = prompt('Ingrese el nuevo saldo:');

          // Llama al servicio editBalance para actualizar el saldo de la tarjeta
          await FinancialServiceEdit.editBalance(cardId, newBalance);

          // Actualiza la lista de tarjetas después de editar el saldo
          this.getUserCards();
          
          console.log('Saldo actualizado con éxito');
          this.successMessage = "Saldo actualizado con éxito";
          this.showSuccessMessage = true;
        } catch (error) {
          console.error('Error al actualizar el saldo:', error.message);
          // Manejo de errores
        }
      },
      selectOption(option) {
        
        this.selectedOption = option;
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
  