<template>
    <div>
        <main class="barraHorizontal">
            <header class="header">
                <div id="menu-btn" class="material-symbols-outlined" @click="toggle">
                    menu
                </div>
                <a data-aos="zoom-in-left" data-aos-delay="150" @click="redirectToPaginaPrincipal" class="logo">
                    <i class="fa-solid fa-paper-plane"></i>AirTravel
                </a>
                <nav class="navbar">
                    <!-- LINKS DE NAVEGACIÓN -->
                    <a data-aos="zoom-in-left" data-aos-delay="300" @click="redirectToPaginaPrincipal" >Inicio</a>
                    <a data-aos="zoom-in-left" data-aos-delay="450" @click="redirectToCheckIn"  v-if="userRole !== 1 && userRole !== 2">Confirmar Check-in</a>
                    <a data-aos="zoom-in-left" data-aos-delay="600" @click="redirectToAyuda">ayuda</a>
                </nav>
                <a data-aos="zoom-in-left" data-aos-delay="600" @click="redirectToCarrito" class="carrito" id="carrito" v-if="userRole === 3 ">
                    <i class="material-symbols-outlined">shopping_cart</i>
                </a>

                <a data-aos="zoom-in-left" data-aos-delay="600" @click="handleUserIconClick"   class="usuario" id="usuario">
                    <i class="material-symbols-outlined">person</i>
                </a>

                <!-- Menú desplegable del usuario -->
                <div class="menuUsuario" :class="{ active: isMenuUsuarioActive }">
                    <ul v-if="userRole === 1">
                        <!-- Root -->
                        <li><a href="/Perfil">Perfil</a></li>
                        <li><a href="/Ad_Management">Gestionar Administradores</a></li>
                        <li>
                            <div class="btn-cerrar" @click="logout">
                                <span class="material-symbols-outlined">logout</span>Cerrar sesión
                            </div>
                        </li>
                    </ul>
                    <ul v-else-if="userRole === 2">
                        <!-- Administrador -->
                        <li><a href="/Perfil">Perfil</a></li>
                        <li><a href="/ListVuelos_Ad">Gestionar Vuelos</a></li>
                        <li><a href="/ListOfertasAdmin">Gestionar Promociones</a></li>
                        <li> <div class="btn-cerrar" @click="logout">
                                <span class="material-symbols-outlined">logout</span>Cerrar sesión
                            </div></li>
                    </ul>
                    <ul v-else-if="userRole === 3">
                        <!-- Cliente -->
                        <li><a href="/Perfil">Perfil</a></li>
                        <li><a href="/M_Financiero">Módulo Financiero</a></li>
                        <li><a href="/Checkin">Check-in</a></li>
                        <li><a href="/List_Reservas">Reservas</a></li>
                        <li v-if=" SuscritoNoticias === 1"><a href="/PromocionesUsuario">Noticias</a></li>
                        <li> <div class="btn-cerrar" @click="logout">
                                <span class="material-symbols-outlined">logout</span>Cerrar sesión
                            </div></li>
                    </ul>
                </div>
            </header>
        </main>
    </div>
</template>

<style lang="scss" scoped>
* {
    //La regla * selecciona todos los elementos de la página y les aplica los estilos CSS
    border: 1px solid black;

    margin: 0;
    padding: 0;
    box-sizing: border-box;
    outline: none;
    border: none;
    text-decoration: none;

    transition: all 0.2s linear;
}

$blue: #54b2f1;

$verde:  #4caf50;
$border: 0.2rem solid $blue;
$gris: #f7f7f7;
$azul: #0d629b;
$blanco: #ffffff;
$negro: #1a1320;
$accent: #0b97f4;
$secondary: #a7d6f6;

//BARRA DE NAVEGACIÓN----------------------------------------------------------------

.header {
    position: fixed; //La barra de navegación se fija en la parte superior de la página
    top: 0;
    right: 0;
    left: 0;
    z-index: 1000;
    background: $blue; //Color de fondo de la barra de navegación
    display: flex;
    align-items: center;
    margin: auto;
    justify-content: space-between;
    padding: 1.5rem 4%;
    box-shadow: 0 3px 6px rgba(0, 0, 0, 0.3);

    .navbar a {
        //Estilos para los enlaces de la barra de navegación
        font-size: 1.7rem;
        position: flex;
        color: $blanco; //Color de letra de los enlaces
        display: inline-block; //Los enlaces se muestran en una sola línea
        margin: 0 5rem; //Establece el margen del elemento en 0 en la parte superior e inferior, y 1rem en la izquierda y derecha.
        transition: 0.2s ease-out;
        font-weight: 450;
        text-transform: capitalize;
        text-shadow: 0 3px 6px rgba(0, 0, 0, 0.2);

        &:hover,
        &.router-link-exact-active {
            color: $negro; //Al pasar el cursor sobre la letra del enlace, el color cambia a azul
            background-color: transparent; //color de fondo al selleccionar el texto
            text-decoration-line: none;
            cursor: pointer; // Cambia el cursor a un puntero al pasar sobre el botón
        }

        &.router-link-exact-active {
            border-right: 5px solid $blanco;
            text-decoration-line: none;
        }
    }

    .navbar {
        display: flex;
        align-items: center;
        justify-content: center; // Centra los enlaces en la barra de navegación
        flex-grow: 1; // Hace que los enlaces ocupen todo el espacio disponible
    }

    .carrito,
    .usuario {
        color: $negro;
        margin: 0 1rem; // Ajusta el margen derecho para separar el usuario del carrito de compras
        background: transparent;

        &:hover,
        &.router-link-exact-active {
            color: $blanco; //Al pasar el cursor sobre la letra del enlace, el color cambia a azul
            background-color: transparent; //color de fondo al selleccionar el texto
            text-decoration-line: none;
            cursor: pointer; // Cambia el cursor a un puntero al pasar sobre el botón
        }

        i {
            //ICONOS DE CARRITO Y USUARIO
            font-size: 3rem; //Tamaño del icono de carrito y usuario
        }
    }


    .menuUsuario.active {
        visibility: visible;
        opacity: 1;
    }
    
    .menuUsuario {
        position: absolute;
        top: 100%;
        /* Coloca el menú justo debajo del ícono del usuario */
        right: 0;
        background-color: $blanco;
        /* Color de fondo del menú del usuario */
        border-radius: 15px;
        width: 200px;
        box-sizing: border-box;
        /* Corrige la propiedad box-sizing */
        visibility: hidden;
        opacity: 0;
        transition: visibility 0s, opacity 0.5s linear;
        /* Transición para la visibilidad y opacidad */
        align-items: center;

        ul {
            list-style: none;
            padding: 0;
            margin: 0;

            li {
                padding: 10px;
                display: flex;
                align-items: center;

                a {
                    text-decoration: none;
                    color: $azul;
                    background-color: transparent;
                    font-weight: 500;
                    transition: color 0.3s;
                    /* Transición para el cambio de color */

                    &:hover {
                        color: $verde;
                        /* Cambio de color al pasar el mouse */
                    }
                }

                .btn-cerrar {
                    text-decoration: none;
                    color: $azul;
                    margin-right:0.5rem ;
                    background-color: transparent;
                    font-weight: 500;
                    transition: color 0.3s;
                    /* Transición para el cambio de color */

                    &:hover {
                        color: $verde;
                        /* Cambio de color al pasar el mouse */
                    }
                }
            }
        }
    }

}

//LOGO----------------------------------------------------------------
.logo {
    //NOMBRE DE LA EMPRESA : AirTravel
    font-size: 2.5rem;
    color: $negro;
    font-weight: bolder;
    text-decoration: none;
    background-color: transparent;

    i {
        //LOGO DE AIRTRAVEL
        color: $blanco;
        padding-right: 0.5rem;
        text-decoration: none;
        background-color: transparent;
        text-shadow: 0 3px 3px rgba(0.4, 0, 0.4, 0.5);
    }

    &:hover,
    &.router-link-exact-active {
        background-color: transparent;
        text-decoration-line: none;
    }
}

//BARRA RESPONSIVE ----------------------------------------------------------------
#menu-btn {
    //BOTÓN DE TRES LINEAS PARA RESPONSIVE
    font-size: 2.5rem; // Tamaño de fuente de 2.5rem
    color: $negro; // Color de texto blanco
    cursor: pointer; // Cambia el cursor a un puntero al pasar sobre el botón
    display: none; // El botón se muestra en la página
    text-decoration: none;
}

@media (max-width: 768px) {
    //Pagina en tamaño de tablet

    #menu-btn {
        display: inline-block; //El menú se muestra desplegado de forma horizontal
    }

    .header {
        .navbar {
            position: absolute;
            top: 99%;
            left: 0;
            right: 0;
            background: $azul; //Color de la barra al desplegarla con el botón de tres lineas
            display: flex;
            /*
                * El siguiente código establece un clip-path para un elemento con forma de polígono.
                * Inicialmente, el clip-path se establece en un rectángulo sin altura, lo que resulta en un elemento no visible.
                * Cuando se agrega la clase 'active' al elemento, el clip-path se cambia a un rectángulo con una altura del 100%,
                * lo que resulta en que el elemento se vuelva visible.
                */
            clip-path: polygon(0 0, 100% 0, 100% 0, 0 0);

            &.active {
                clip-path: polygon(0 0, 100% 0, 100% 100%, 0 100%);
            }

            a {
                margin: 2rem;
                font-size: 2rem;
                display: block;
            }
        }

        .carrito {
            position: absolute;
            cursor: pointer; // Cambia el cursor a un puntero al pasar sobre el botón
            right: 6.5rem; // Ajusta el margen derecho según sea necesario
        }
    }
}

@media (max-width: 450px) {

    //pagina de tamaño de celular
    //Se muestra botón de tres lineas con menú
    html {
        font-size: 50%;
    }

    .header {
        justify-content: space-between;
        padding: 1.5rem 1rem;

        .navbar {
            position: absolute;
        }

        .carrito {
            position: absolute;
            cursor: pointer; // Cambia el cursor a un puntero al pasar sobre el botón
            right: 6.5rem; // Ajusta el margen derecho según sea necesario
        }
    }
}


</style>

<script>

import logoutService from "@/services/authenticationService/logoutService.js";
export default {

    data() {
        return {
            token: window.sessionStorage.getItem("JWTtoken"),
            isMenuUsuarioActive: false,
            userRole: null, // Almacena el rol del usuario
            SuscritoNoticias:null,
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
        };
    },
    mounted() {

        this.getUserRole(); // Llama a la función para obtener el rol del usuario

    },

    methods: {
        handleUserIconClick() {
            const token = window.sessionStorage.getItem("JWTtoken");
            if (token && token !== null) {
                this.isMenuUsuarioActive = !this.isMenuUsuarioActive;
                this.getUserRole(); // Llama a la función para obtener el rol del usuario
            } else {
                this.redirectToLogin();
            }
        },
        redirectToLogin() {
            this.$router.push("/Login"); // Redirige a la página de inicio de sesión
        },
        getUserRole() {
            const token = window.sessionStorage.getItem("JWTtoken");
            if (this.token) {
                const tokenData = JSON.parse(atob(token.split('.')[1]));

                if (tokenData.role == "root") {
                    this.userRole = 1;
                }
                if (tokenData.role == "admin") {
                    this.userRole = 2;
                }
                if (tokenData.role == "registeredUser") {
                    this.userRole = 3;
                }
              
            }
            

        },

        toggle() {
            let navbar = document.querySelector(".navbar");
            navbar.classList.toggle("active");
        },
        redirectToPaginaPrincipal() {
            this.$router.push("/");
        },
        redirectToCheckIn() {
            this.$router.push("/Checkin");
        },
        redirectToAyuda() {
            this.$router.push("/CentroAyuda");
        },
        redirectToCarrito() {
            this.$router.push("/Carrito");
        },

        logout(){
            this.isMenuUsuarioActive = false; // Cierra el menú desplegable del usuario
            this.userRole=null;
            // Limpiar sessionStorage
            window.sessionStorage.clear();
            logoutService.logout().then((response) => {

            // Maneja la respuesta exitosa aquí
                if (response.status === 200) {
                    console.log("logout exitoso", response.data);
                }
            })
            .catch((error) => {
                console.error("Something happened:", error);
                this.errorMessage = error.response.data.message || "Something happened, try to logout and login again please";
                this.showErrorMessage = true;
            }
            );
            // Remove the JWT token from the localStorage
            window.sessionStorage.removeItem("JWTtoken");
            this.$router.push("/Login");
         
        },

    },
};
</script>
