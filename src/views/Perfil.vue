<template>
  
        <spinner :showSpinner="showSpinner"></spinner>
        <div class="card card-large light-style flex-grow-1 container-p-y">
            <div class="row no-gutters row-bordered row-border-light">
                <div class="col-md-2 pt-0">
                    <div class="list-group list-group-flush account-settings-links">
                        <a class="list-group-item list-group-item-action active" data-toggle="list"
                            href="#account-general">Información Personal</a>
                        <a class="list-group-item list-group-item-action" data-toggle="list"
                            @click="redirectToUpdatePassword">Cambiar Contraseña</a>
                        <a class="list-group-item list-group-item-action" data-toggle="list" @click="redirectToRootIDChange"
                            v-if="isRoot">Editar ID del root</a>
                       

                    </div>
                </div>
                <div class="col-md-9">
                    <div class="tab-content">
                        <div class="tab-pane fade active show" id="account-general">
                            <div class="card-body align-items-center">
                                <img :src=" profile.profileImage" class="imagenPerfil" alt="Imagen de perfil" width="100"
                                    height="100">
                            </div>
                            <hr class="border-light m-0">
                            <div class="card-body">
                                <div class="form-group ">
                                    <label class="form-label">Usuario</label>
                                    <input type="text" class="form-control" v-model="profile.username" required readonly>
                                </div>
                                <div class="form-group">
                                    <label class="form-label">Nombre</label>
                                    <input type="text" class="form-control" v-model="profile.firstName" required readonly>
                                </div>
                                <div class="form-group">
                                    <label class="form-label">Apellido</label>
                                    <input type="text" class="form-control" v-model="profile.lastName" required readonly>
                                </div>
                                <div class="form-group">
                                    <label class="form-label">Correo Electrónico</label>
                                    <input type="text" class="form-control mb-1" v-model="profile.email" required readonly>
                                    <!-- <div class="alert alert-warning mt-3">
                                            Tu correo no ha sido confirmado. Verifica tu bandeja de entrada.<br>
                                            <a href="javascript:void(0)">Reenviar confirmación</a>
                                        </div> -->
                                </div>


                                <div class="form-group">
                                    <label class="form-label">Fecha de Nacimiento</label>
                                    <input type="text" class="form-control" v-model="formattedBirthday" required readonly>
                                </div>
                                <div class="form-group">
                                    <label class="form-label">Lugar de Nacimiento</label>
                                    <input type="text" class="form-control" v-model="profile.birthPlace" required readonly>
                                </div>

                                <div class="form-group">
                                    <label class="form-label">Dirección de Facturación</label>
                                    <input type="text" class="form-control" v-model="profile.billingAddress" required
                                        readonly>
                                </div>
                                <div class="form-group">
                                    <label class="form-label">DNI</label>
                                    <input type="text" class="form-control" v-model="profile.dni" required readonly>
                                </div>
                                <div class="form-group">
                                    <label class="form-label">Género</label>
                                    <input class="form-control" v-model="profile.gender" required readonly>
                                </div>

                                <div class="form-group">
                                    <div class="switch-button">
                                        <label class="form-label">Suscribirse al módulo de noticias</label>
                                        <!-- Checkbox -->
                                        <input type="checkbox" name="switch-button" id="switch-label"
                                            class="switch-button__checkbox" v-model="profile.subscribedToFeed" disabled
                                            required />
                                        <!-- Botón -->
                                        <label for="switch-label" class="switch-button__label"></label>
                                    </div>
                                </div>
                            </div>
                            <div class="text-right mt-3 bt-3">
                                <button @click="updateProfile" class="btn btn-primary">Editar</button>&nbsp;
                            </div>
                        </div>
                    </div>
                </div>

            </div>

            <div style="margin-bottom: 50px;"></div>
        </div>
        <!------------------------------------------------FOOTER------------------------------------------->
       
        <error-modal :show-error="showErrorMessage" :error-message="errorMessage" @close="showErrorMessage = false" />
        
    <!------------------------------------------------FOOTER------------------------------------------->
      <!-- Footer aquí -->
      <Footer></Footer>
</template>
<style lang="scss"  >
$light-color: #312c02;
$degradado: rgba(149, 168, 238, 0.11);
$bg: rgba(6, 31, 14, 0.947);
$azul-claro: #CFE0EB;
$gris: #F7F7F7;
$gris2: #364265;
$verde: #00BD8E;
$azul: #0D629B;
$blue: #54b2f1;
$blanco: #FFFFFF;
$negro: #1A1320;
$accent: #0B97F4;
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
.container, .footer {
  width: 100%; /* Esto hace que el contenedor y el footer ocupen el ancho completo de su contenedor padre */
}


@mixin grid($val) {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax($val, 1fr));
    gap: 1.5rem;
}

/* Estilo para pantallas pequeñas */
@media (min-width: 500px) {
    .ui-w-80 {
        width: 30% !important;
        /* Toma el ancho completo en pantallas pequeñas */
    }

    .card {
        margin-right: -5rem;
        /* Márgenes más amplios en pantallas de laptop */
        width: 100%;
    }
}

@media (min-width: 700px) {
    .ui-w-80 {
        width: 30% !important;
        /* Toma el ancho completo en pantallas pequeñas */
    }

    .card {
        margin-right: -1rem;
        /* Márgenes más amplios en pantallas de laptop */
        width: 80%;
    }


}

/* Estilo para pantallas de laptop (por ejemplo, 15.6 pulgadas) */
@media (min-width: 992px) {
    .ui-w-80 {
        width: 8rem !important;
    }

    .card {

        width: 100%;
        margin-right: -5rem;
    }
}

@media (min-width: 1024px) {
    .ui-w-80 {
        width: 8rem !important;
    }

    .card {
        width: 100%;
        /* Márgenes más amplios en pantallas de laptop */
        margin-left: -1rem;

    }
}

.d-flex {
  display: flex;
}
.list-group-item.list-group-item-action.active {
  color: black;
  align-items: center;
}
list-group-item list-group-item-action {
  color: black;
  align-items: center;
  align-content: center;
}
.align-items-center {
  align-items: center;
}

.justify-content-center {
  justify-content: center;
}

.ui-w-80 {
    width: 8rem !important;
}


.btn-default {
    border-color: rgba(24, 28, 33, 0.1);
    background: rgba(195, 173, 173, 0);
    color: #222426;
}



label.btn {
    margin-bottom: 10;
}

.btn-outline-primary {
    border-color: $accent;
    background: transparent;
    color: $azul;
}



.btn {
    cursor: pointer;
}

.text-light {
    color: $azul !important;
}

.imagenPerfil {
    display: block !important; // Asegura que la imagen esté centrada en su contenedor
    border-radius: 50%;
    /* Aplicamos un borde circular */
    overflow: hidden;
    /* Aseguramos que la imagen esté dentro del círculo */
    align-items: center !important;
    margin: 0 auto 2rem; // Alinea la imagen al centro horizontalmente dentro de su contenedor
    display: flex !important;
    justify-content: center !important;
}


.card {
    background-clip: padding-box;
    box-shadow: 0 6px 6px rgba(6, 6, 6, 0.1);
    background: #3B5998;
    background: $secondary; //FONDO DE LA CARD
    margin-top: 10rem;
    width: 90vw;
    margin-right: -10%;

    .btn-cerrar {
        //Botón de cerrar sesión 
        background-color: $gris2;
        color: $blanco;
        margin-top: 1rem;
        margin: 1rem auto; //Centrado 
        display: inline-block;
        padding: 1rem 3rem;
        font-size: 1.7rem;
        border: $azul-claro .2rem solid;
        border-radius: 5rem;
        box-shadow: inset 0px 0px 0px 1px $negro;
    }
 
}

.row-bordered {
    overflow: hidden;
}

.account-settings-fileinput {
    position: absolute;
    visibility: hidden;
    width: 1rem;
    height: 1rem;
    opacity: 0;
    
}

.account-settings-links .list-group-item.active {
    font-weight: bold !important;
    
   
}

html:not(.dark-style) .account-settings-links .list-group-item.active {
   background: transparent !important;
    
}

.account-settings-multiselect~.select2-container {
    width: 100% !important;
}

.light-style .account-settings-links .list-group-item {//No seleccionados
    padding: 0.85rem 1.5rem;
    border-color: rgba(24, 28, 33, 0.03) !important;
    font-weight: bold !important;
    
}


.light-style .account-settings-links .list-group-item.active {
    color: #0c0c0c !important;
    border: 1px solid #1363ae !important;
    border-radius: 1px !important;
    box-shadow: 0px 0px 5px rgba(19, 99, 174, 0.5) !important;
   // background-color: #f5f5f5 !important;
    transition: all 0.3s ease-in-out !important;
    
}



.light-style .account-settings-links .list-group-item {
    padding: 0.85rem 1.5rem;
    border-color: rgba(24, 28, 33, 0.03) !important;
    
    
}


/* Aumenta el tamaño de las etiquetas en los formularios */
.form-label {
    font-size: 1.8rem;
    /* Ajusta el tamaño según tus preferencias */
}

.form-group {
    font-size: 1.5rem;

    /* Aumenta el tamaño de los campos de entrada según tus preferencias */
    input {
        font-size: 1.5rem;
        /* Aumenta el tamaño de los campos de entrada según tus preferencias */
    }

    option {
        font-size: 1.5rem;
        /* Aumenta el tamaño de los campos de entrada según tus preferencias */
    }
}

.custom-select {
    font-size: 1.5rem;
    /* Aumenta el tamaño de los campos de entrada según tus preferencias */
}

.text-right .btn {
    margin-right: 1rem;
    /* Ajusta este valor según tus preferencias */
    padding: 0.5rem 2rem;
    /* Ajusta el relleno según tus preferencias */
}


//BOTÓN DESLIANTE 
.switch-button {
    display: inline-block;

}

.switch-button .switch-button__checkbox {
    display: none;
}

.switch-button .switch-button__label {
    //BOTÓN INACTIVO
    background-color: $azul; //Color de fondo del botón deslizante
    width: 5rem;
    height: 3rem;
    border-radius: 3rem;
    display: inline-block;
    position: relative;


}

.switch-button .switch-button__label:before {
    transition: .2s;
    display: block;
    position: absolute;
    width: 3rem;
    height: 3rem;
    background-color: $blanco; //Color del circulo deslizante
    content: '';
    border-radius: 50%;
    box-shadow: inset 0px 0px 0px 1px $negro;

}

.switch-button .switch-button__checkbox:checked+.switch-button__label {
    background-color: $verde; //Color de fondo del botón deslizante
}

.switch-button .switch-button__checkbox:checked+.switch-button__label:before {
    transform: translateX(2rem); //Movimiento del circulo blanco 

}

.footer {
    .box-container {
        @include grid(25rem);
        // background: #364265;
        background: #0d629b;
        box-shadow: 2px 3px 4px 5px rgba(156, 156, 161, 0.2);
        border-radius: 10px;
        padding: 3rem 2rem;
        margin-top: 10rem;
        width: 90vw;

        .box {
            padding: 1rem 0;
            //background: #364265;
            background: #0d629b;

            .logo {
                color: $blanco;
            }

            h3 {
                font-size: 2.2rem;
                color: $blue;
                font-weight: bolder;
                padding: 1.5rem 0;
                text-transform: capitalize;
                text-shadow: 1px 3px 3px rgba(1, 0.4, 0.3, 0.6);

            }

            p {
                font-size: 1.4rem;
                color: $blanco;
                padding: 0.5rem 0;
                line-height: 2;

                i {
                    //Iconos de informacion de contacto
                    padding-right: .5rem;
                    color: $negro;
                    text-shadow: 1px 3px 3px rgba(1, 0.4, 0.3, 0.6);
                }


            }


            .links {
                //Links rapidos a secciones de la pagina
                font-size: 1.4rem;
                color: $blanco;
                padding: 1rem 0;
                display: block;
                text-transform: capitalize;
                font-weight: bolder;

                z {
                    background-color: transparent;
                    color: $verde;
                    text-decoration: none;

                    i {
                        padding-right: 2rem;
                    }
                }

                i {
                    padding-right: .5rem;
                    color: $negro;
                }
            }

            form .email {
                width: 100%;
                border-radius: 5rem;
                border: $blue solid .1rem;
                background: none;
                font-size: 1.5rem;
                text-transform: none;
                color: $blanco;
                margin-top: 1rem;
                padding: 1.4rem 1.4rem;

                &::placeholder {
                    color: $blanco;
                }


            }

        }

    }
}

.credit {
    //Autores de la pagina
    background: $gris2;
    box-shadow: 3px 3px 6px rgba(1, 0.2, 1, 0.2);
    border-radius: 10px;
    text-align: center;
    font-size: 2rem;
    padding: 2rem 1rem;
    color: $accent;
    font-weight: bolder;
    margin-top: 5rem;

    span {
        color: $blanco;
    }
}
</style>
<script>
import logoutService from "@/services/authenticationService/logoutService.js";
import { format } from 'date-fns'; // Importa la función de formato de date-fns
import updateProfileService from "@/services/userService/updateProfileService.js";
import viewProfileService from "@/services/userService/viewProfileService.js";
import errorModal from "@/components/errorModal.vue";
import spinner from "@/components/spinner.vue";
import Footer from '@/components/footer.vue';

export default {
    data() {
        return {
            isRoot: false,
            hasCreateAdminPermission: false,
            showSpinner: false, // Initialize as hidden
            profile: {
                id: "",
                email: "",
                dni: "",
                firstName: "",
                lastName: "",
                birthday: "",
                birthPlace: "",
                billingAddress: "",
                gender: "",
                role: "",
                username: "",
                password: "",
                profileImage: "",
                active: "",
                subscribedToFeed: "",
            },
            isEditing: {
                id: "",
                email: "",
                dni: "",
                firstName: "",
                lastName: "",
                birthday: "",
                birthPlace: "",
                billingAddress: "",
                gender: "",
                role: "",
                username: "",
                password: "",
                profileImage: "",
                active: "",
                subscribedToFeed: "",
            },

            originalProfile: {}, // To store the original profile before editing
            errorMessage: "",
            showErrorMessage: false,
        };
    },
    computed: {
        formattedBirthday() {
            // Formatea la fecha en un formato legible (por ejemplo, 'dd/MM/yyyy')
            return this.profile.birthday ? format(new Date(this.profile.birthday), 'yyyy-MM-dd') : '';
        },
    },
    created() {
        this.showSpinner = true;
        // Create a Date object from the Unix timestamp

        // Get the user ID from the JWT token in sessionStorage
        const token = window.sessionStorage.getItem('JWTtoken');
        const tokenData = JSON.parse(atob(token.split('.')[1]));
        const id = tokenData.ID;
        if (tokenData.role == "root") {
            this.isRoot = true;
        }
        const permissions = tokenData.permissions;
        if (permissions.includes("create_admin")) {
            this.hasCreateAdminPermission = true;
        }

        // Fetch user data and populate the profile object
        viewProfileService.viewProfile(id)
            .then(response => {
                this.profile = response.data;
                if (response.status == 200) {
                    this.showSpinner = false;
                    console.log("User Profile", response.data);
                    // You can redirect the user or perform other actions here.
                }
            })
            .catch(error => {
                this.showSpinner = false;
                // Handle login errors here
                if (error.response.status == 403) {
                    console.log("User not found sorry:", error.response.status, error);
                    this.errorMessage = error.response.data.message || "User not found sorry";
                    this.showErrorMessage = true;
                }
                else {
                    // You can redirect the user or perform other actions here.
                    console.error("Something happened:", error);
                    this.errorMessage = error.response.data.message || "Something happened";
                    this.showErrorMessage = true;
                }
                // Display an error message to the user or take appropriate action.
                console.error('Error fetching user data:', error);
                this.errorMessage = error.response.data.message || "Error en el fetching, por favor cierre sesión y vuelva a iniciarla";
                this.showErrorMessage = true;
            });
        },
    methods: {
        redirectToAdminManagement() {
            this.$router.push('/Ad_Management');
        },
        redirectToUpdatePassword() {
            this.$router.push('/ResetPassword');
        },
        redirectToRootIDChange() {
            this.$router.push('/CambioIdRoot');
        },

        toggleEdit(field) {
            this.isEditing[field] = !this.isEditing[field];
            if (this.isEditing[field]) {
                // Save the original value before editing
                this.originalProfile[field] = this.profile[field];
            } else {
                // Restore the original value if editing is canceled
                this.profile[field] = this.originalProfile[field];
            }
        },
        updateProfile() {
            const token = window.sessionStorage.getItem("JWTtoken");
            const tokenData = JSON.parse(atob(token.split('.')[1]));

            // Assuming the token contains a field named 'id' with the user's ID
            const id = tokenData.ID;
            // Here, you can implement the logic to save changes to the backend or perform any necessary actions.
            // For now, we'll just disable editing.


            updateProfileService.updateProfile(id, this.profile.email, this.profile.dni, this.profile.firstName, this.profile.lastName, this.profile.birthday, this.profile.birthPlace, this.profile.billingAddress, this.profile.gender, this.profile.role, this.profile.username, this.profile.profileImage, this.profile.active, this.profile.subscribedToFeed)
                .then(response => {

                    // Handle success
                    if (response.status == 200) {
                        console.log("User Profile!!", response.data);
                        this.$router.push('/EditarPerfil'); //CLICK EN EDITAR PERFIL
                        // You can redirect the user or perform other actions here.
                    }
                })
                .catch(error => {

                    // Handle login errors here
                    if (error.response.status == 403) {
                        console.log("User not found sorry:", error.response.status, error);
                        this.errorMessage = error.response.data.message || "User not found";
                        this.showErrorMessage = true;
                    }
                    else {
                        // You can redirect the user or perform other actions here.
                        console.error("Something happened:", error);
                        this.errorMessage = error.response.data.message || "Something happened";
                        this.showErrorMessage = true;
                    }
                    // Display an error message to the user or take appropriate action.
                    console.error('Error fetching user data:', error);
                    this.errorMessage = error.response.data.message || "Error fetching user data, logout and login again";
                    this.showErrorMessage = true;
                });

            Object.keys(this.isEditing).forEach((field) => {
                this.isEditing[field] = false;
            });
        },
        cancelChanges() {
            this.showSpinner = true;
            // Cancel editing and revert changes to the original values
            Object.keys(this.isEditing).forEach((field) => {
                this.isEditing[field] = false;
                this.profile[field] = this.originalProfile[field];
            });
            this.showSpinner = false;
        },

    },
    components: {
        errorModal,
        spinner,
        Footer,
    },
};
</script>