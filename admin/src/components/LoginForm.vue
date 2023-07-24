<template>
    <form @submit.prevent="login">
      <BaseInput
        type="email"
        label="Email"
        name="email"
        v-model="email"
        autocomplete="email"
        placeholder="luke@jedi.com"
        class="mb-2"
      />
      <BaseInput
        type="password"
        label="Password"
        name="password"
        v-model="password"
        class="mb-4"
      />
      <div class="flex justify-between">
        <BaseBtn type="submit" text="Login" />
        <router-link to="/forgot-password" class="text-sm base-link">
          Forgot your password?
        </router-link>
      </div>
      <FlashMessage :error="error" />
    </form>
  </template>
  
  <script>
  import { getError } from "@/utils/helpers";
  import BaseBtn from "@/components/BaseBtn";
  import BaseInput from "@/components/BaseInput";
  import AuthService from "@/services/AuthService";
  import FlashMessage from "@/components/FlashMessage";
  
  export default {
    name: "LoginView",
    components: {
      BaseBtn,
      BaseInput,
      FlashMessage,
    },
    data() {
      return {
        email: null,
        password: null,
        error: null,
      };
    },
    methods: {
      async login() {
        const payload = {
          email: this.email,
          password: this.password,
        };
        this.error = null;
        try {
          // Effectuer la requête POST vers l'API de connexion du backend Laravel
          const response = await AuthService.login(payload);
  
          // Vérifier la réponse de l'API
          if (response.data.success) {
            // La connexion a réussi, vous pouvez rediriger l'utilisateur vers la page de tableau de bord ou toute autre page appropriée
            this.$router.push("/dashboard");
          } else {
            // La connexion a échoué, affichez un message d'erreur
            this.error = "Identifiants invalides, veuillez réessayer.";
          }
        } catch (error) {
          // Gérer les erreurs d'API ici
          this.error = "Une erreur s'est produite lors de la connexion, veuillez réessayer.";
        }
      },
    },
  };
  </script>
  