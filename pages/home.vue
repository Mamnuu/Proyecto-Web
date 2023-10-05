<template>
    <h1>HOME</h1>
</template>
<script setup>
import Swal from "sweetalert2";
//Alerta de inicio de sesion existoso
const Toast = Swal.mixin({
  toast: true,
  position: 'top-end',
  showConfirmButton: false,
  timer: 3000,
  timerProgressBar: true,
  didOpen: (toast) => {
    toast.addEventListener('mouseenter', Swal.stopTimer)
    toast.addEventListener('mouseleave', Swal.resumeTimer)
  }
})

Toast.fire({
  icon: 'success',
  title: 'Inicio de sesión exitoso'
})
</script>