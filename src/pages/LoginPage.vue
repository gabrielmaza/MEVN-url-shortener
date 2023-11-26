<script setup>
// import { useQuasar } from "quasar";
import { useRouter } from "vue-router";
import { ref } from "vue";
import { useUserStore } from "src/stores/user-store";

// const $q = useQuasar();

// const alertDialogBackend = (message = "Error en el servidor...") => {
//   $q.dialog({
//     title: "Alert",
//     message: message,
//   });
// };

const userStore = useUserStore();
const router = useRouter();
const email = ref("andres@test.com");
const password = ref("123123");

const handleSubmit = async () => {
  try {
    await userStore.access(email.value, password.value);
    router.push("/");
    email.value = "";
    password.value = "";
  } catch (error) {
    console.log("error", error);
    // alertDialogBackend(error.error);
  }
};
</script>

<template>
  <p-page class="row justify-center">
    <div class="col-12 col-sm-6 col-md-5">
      <h3>Login page</h3>
      <q-form @submit.prevent="handleSubmit">
        <q-input
          v-model="email"
          label="Ingrese un email"
          type="text"
          :rules="[
            (val) =>
              (val && /^[^@]+@[^@]+\.[a-zA-Z]{2,}$/.test(val)) ||
              'Ingresa un email válido',
          ]"
        ></q-input>
        <q-input
          v-model="password"
          label="Ingrese contraseña"
          type="password"
          :rules="[
            (val) =>
              (val && val.length > 5) ||
              'Ingresa una contraseña de por lo menos 6 caracteres',
          ]"
        ></q-input>
        <div>
          <q-btn label="Login" type="submit"></q-btn>
        </div>
      </q-form>
    </div>
  </p-page>
</template>
