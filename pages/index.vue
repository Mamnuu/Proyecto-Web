<template>
    <div class="container" v-bind:class="{ 'sign-up-mode': signupMode }">
        <div class="forms-container">
            <div class="signin-signup">
                <form action="#" class="sign-in-form">
                    <h2 class="title">Bienvenido</h2>
                    <h2 class="title">Ingresa aquí</h2>
                    <div class="input-field">
                        <i class="fas fa-user"></i>
                        <input type="text" placeholder="Nombre de usuario" />
                    </div>
                    <div class="input-field">
                        <i class="fas fa-lock"></i>
                        <input type="password" placeholder="Contraseña" />
                    </div>
                    <input type="submit" value="Ingresar" class="btn solid" />
                    <!-- <p class="social-text">Or Sign in with social platforms</p>
                    <div class="social-media">
                        <a href="#" class="social-icon">
                            <i class="fab fa-facebook-f"></i>
                        </a>
                        <a href="#" class="social-icon">
                            <i class="fab fa-twitter"></i>
                        </a>
                        <a href="#" class="social-icon">
                            <i class="fab fa-google"></i>
                        </a>
                        <a href="#" class="social-icon">
                            <i class="fab fa-linkedin-in"></i>
                        </a>
                    </div> -->
                </form>
                <!-- <form action="#" class="sign-up-form">
                    <h2 class="title">Regístrate</h2>
                    <div class="input-field">
                        <i class="fas fa-user"></i>
                        <input type="text" placeholder="Nombre de usuario" />
                    </div>
                    <div class="input-field">
                        <i class="fas fa-envelope"></i>
                        <input type="email" placeholder="Correo" />
                    </div>
                    <div class="input-field">
                        <i class="fas fa-lock"></i>
                        <input type="password" placeholder="Contraseña" />
                    </div>
                    <input type="submit" class="btn" value="Sign up" /> -->
                    <!-- <p class="social-text">Or Sign up with social platforms</p>
                    <div class="social-media">
                        <a href="#" class="social-icon">
                            <i class="fab fa-facebook-f"></i>
                        </a>
                        <a href="#" class="social-icon">
                            <i class="fab fa-twitter"></i>
                        </a>
                        <a href="#" class="social-icon">
                            <i class="fab fa-google"></i>
                        </a>
                        <a href="#" class="social-icon">
                            <i class="fab fa-linkedin-in"></i>
                        </a>
                    </div> -->
                <!-- </form> -->
            </div>
        </div>

        <div class="panels-container">
            <div class="panel left-panel">
                <div class="content">
                    <h3>Lorem ipsum</h3>
                    <p>
                        Lorem ipsum, dolor sit amet consectetur adipisicing elit. Debitis,
                        ex ratione. Aliquid!
                    </p>
                    <!-- <button class="btn transparent" id="sign-up-btn" v-on:click="toggleSignupMode">
                        Regístrate
                    </button> -->
                </div>
                <img src="./images/log.svg" class="image" alt="" />
            </div>
            <!-- <div class="panel right-panel">
                <div class="content">
                    <h3>Ya eres miembro ?</h3>
                    <p>
                        Lorem ipsum dolor sit amet consectetur adipisicing elit. Nostrum
                        laboriosam ad deleniti.
                    </p>
                    <button class="btn transparent" id="sign-in-btn" v-on:click="toggleSignupMode">
                        Ingresar
                    </button>
                </div> -->
                <!-- <img src="./images/register.svg" class="image" alt="" /> -->
            <!-- </div> -->
        </div>
    </div>
</template>

<style  src="./styles.index.css"></style>

<script setup>
let signupMode = ref(false)

function toggleSignupMode() {
    signupMode.value = !signupMode.value;
}
</script>
