<template>
    <div class="admin-container">
      <spinner :showSpinner="showSpinner"></spinner>
      <div class="admin-info-container">
        <hr>
        <h1 class="title">Crear administrador</h1>
        <form @submit.prevent="newAdmin" class="inputs-container">
          <hr>
          <p class="texto">Por favor ingrese Correo Electrónico y nombre del administrador</p>
          <input class="input-admin" type="email" id="email" placeholder="Email" v-model="email" required>
          <input class="input-admin" id="usuario" type="text" placeholder="Usuario" v-model="firstName" required>
          <!-- <input class="input-admin" id="password" type="password" placeholder="Password" v-model="password" required> -->
          <button class="btn-admin" type="submit">Confirmar Creación</button>
        </form>
        <p id="error-message" class="error-message">{{ errorMessage }}</p>
      </div>
      <img class="image-containerCrearAdmin" alt="">
      <error-modal :show-error="showErrorMessage" :error-message="errorMessage" @close="showErrorMessage = false" />
      <success-modal :show-note="showSuccessMessage" :success-message="successMessage" @close="showSuccessMessage = false" />
    </div>
    <!------------------------------------------------FOOTER------------------------------------------->
    <Footer></Footer>

</template>

<style lang="scss" scoped>
   
     $light-color:#312c02;
     $degradado: rgba(149, 168, 238, 0.11);
     $bg:rgba(6, 31, 14, 0.947);
     $azul-claro: #CFE0EB;
     $gris:#F7F7F7;
     $verde: #00BD8E;
     $azul: #0D629B;
     $blanco: #FFFFFF;
     $negro:#1A1320;
     $accent:#0B97F4;
     $secondary:#ceeafd;
     html{
        /* 
        Estilo CSS para la vista Home.vue del proyecto Vue.js. 
        La propiedad font-size establece el tamaño de fuente base en 10px, lo que equivale a 62.5% del tamaño de fuente predeterminado del navegador. 
        La propiedad overflow-x:hidden oculta el desplazamiento horizontal de la página. 
        La propiedad scroll-behavior: smooth agrega un efecto de desplazamiento suave al hacer clic en los enlaces internos de la página. 
        La propiedad scroll-padding-top establece la cantidad de espacio de relleno en la parte superior de la página para compensar la barra de navegación fija. 
        */
        font-size: 62.5%;
        overflow-x:hidden;
        scroll-behavior: smooth;
        scroll-padding-top:9rem;

        /* 
        Estilos para la barra de desplazamiento en la vista Home.vue.
        Se utiliza el selector de pseudo-elemento &:: para aplicar estilos a la barra de desplazamiento.
        */
        &::-webkit-scrollbar{
            width:1rem;
        }

        &::-webkit-scrollbar-track{
            background: $accent;
        }

        &::-webkit-scrollbar-thumb{
            background-color: $azul;
        }

      }

    .admin-container {
        height: 85vh;
        box-shadow: 3px 3px 6px rgba(1, 0.2, 1, 0.2);
      // background-image: linear-gradient(to top, #a18cd1 0%, #fbc2eb 100%);
    }

    .admin-container {
        
        width: 90vw;
        margin: 0 auto; /* Centrar horizontalmente */
        margin-top: 10rem; /* Centrar verticalmente */
        border-radius: 10px;
        display: flex;
        justify-content: space-between;
        overflow: hidden;
    }


    .admin-info-container {
        width: 50%;
        display: flex;
        flex-direction: column;
        align-items: center;
        text-align: center;
        padding-top: 3rem;
        background-color: $secondary;
        .inputs-container {
            height: 55%;
            width: 60%;
            display: flex;
            flex-direction: column;
            justify-content: space-around;
            align-items: center;
        }
    }

    .image-containerCrearAdmin {
        padding: 2rem;
        width: 50%;
        background-color: $azul;
        box-sizing: border-box;
    }

    .title {
        text-transform: capitalize;
        font-size: 2.25rem;
        font-weight: 600;
        letter-spacing: 1px;
        color: $azul;
        
    }

 

    .social-login-element {
        width: 12.5rem;
        height: 3.75rem;
        font-size: 1.2rem;
        font-weight: 700;
        letter-spacing: 0.5px;
        border-radius: 5px;
        border: 1px solid $azul;
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .social-login-element img {
        width: 1.875rem;
        height: 1.875rem;
        position: relative;
        top: 0;
        left: -0.625rem;
    }

   

    .admin-info-container > p {
        font-size: 1rem;
        margin-top: 1rem;
    }

    

    .input-admin {
          width: 100%;
          margin: 10px 0;
          padding: 10px;
          border: 1px solid #ccc;
          border-radius: 5px;

    }

  

    .input-admin:hover {
        border: 2px solid $azul;
    }

    .btn-admin {
        
        margin-top:1rem;
        display:inline-block;
        padding: 1rem 3rem; 
        font-size: 1.7rem;
        color: $accent;
        border:$azul .2rem solid; 
        border-radius:5rem;
        cursor: pointer;
        background: $blanco;
        box-shadow: 1px 5px 5px rgba(0.5, 0.1, 0.1, 0.1);

        &:hover{
            background: $accent;
            color: $blanco;
            text-decoration-line: none;
            
        }
    
    }

    .inputs-container p {
        margin: 0;
        .span {
            color: $accent;
            font-weight: 600;
            cursor: pointer;
         }
    }

    

    @media screen and (max-width: 1000px) {
        .admin-container {
            width: 70%;
            margin-top: 10rem;
        }
        .admin-info-container {
            width: 100%;
            border-radius: 5px;
        }
        
        .image-container {
            display: none;
        }  
    }

    @media screen and (max-width: 650px) {
        .admin-container {
            width: 90%;
        }
    }

    @media screen and (max-width: 500px) {
        .admin-container {
            height: 100%;
            margin-top:10rem;
        }

        .admin-info-container  {
           .texto{
                margin-top:5rem ;
            }
            
            
        }
    }

</style>
<script>
import newAdminService from "@/services/adminService/newAdminService.js";
import errorModal from "@/components/errorModal.vue";
import spinner from "@/components/spinner.vue";
import successModal from "@/components/successModal.vue";
import Footer from '@/components/footer.vue';

export default {
  data() {
    return {
      firstName: "",
      email: "",
      password: "",
      errorMessage: "",
      showErrorMessage: false,
      showSpinner: false, // Initialize as hidden
      successMessage: "",
      showSuccessMessage: false,
    };
  },
  methods: {
    async newAdmin() {
      this.showSpinner = true;
      const { firstName, email, password } = this;

      try {
        const response = await newAdminService.newAdmin(firstName, email, password);

        if (response.status === 200) {
          //confirm("Administrador creado exitosamente");
          this.successMessage =  "Administrador creado exitosamente, correo enviado con contraseña temporal";
          this.showSuccessMessage = true;
          this.showSpinner = false;
          console.log("Creation successful:", response.data);
          this.$router.push('/Ad_Management');
        }
      } catch (error)  {
        this.showSpinner = false;
        if (error.response && error.response.status === 401) {
          console.log("Login failed:", error.response.status, error);
          this.errorMessage =  "Login failed. Error 401";
          this.showErrorMessage = true;
        } else if (error.response && error.response.status === 403) {
          console.log("User not found:", error.response.status, error);
          this.errorMessage = "User not found";
          this.showErrorMessage = true;
        } else {
          console.error("Email already exist", error);
          this.errorMessage = error.response.data.message || "Email already exist";
          this.showErrorMessage = true;
        }
      }
    },
  },
  components: {
    errorModal,
    spinner,
    successModal,
    Footer,
  },
};
</script>