<template>
    <div v-if="!isSuccess">
        <div v-if="hasError" class="row">
            <div class="col-12 col-lg-4 offset-lg-4">
                <div class="alert alert-danger" role="alert">
                    <span class="d-block" v-for="error in errors">{{ error }}</span>
                </div>
            </div>
        </div>
        <div class="row">
            <form method="post" class="col-lg-4 col-12 offset-lg-4 form-signin form-registration">
                <h1 class="h3 mb-3 font-weight-normal">Inscription</h1>
                <label for="inputUsername" class="sr-only">Username</label>
                <b-form-input
                        id="inputUsername"
                        v-model="username"
                        required autofocus
                        size="lg"
                        placeholder="nom d'utilisateur"
                ></b-form-input>

                <label for="inputEmail" class="sr-only">Email</label>
                <b-form-input
                        id="inputEmail"
                        v-model="email"
                        required autofocus
                        size="lg"
                        placeholder="email"
                ></b-form-input>


                <label for="inputPassword" class="sr-only">Password</label>
                <b-form-input
                        id="inputPassword"
                        v-model="password"
                        :type="passwordType"
                        size="lg"
                        required
                        placeholder="mot de passe"
                ></b-form-input>
                <div class="password-shower float-right" @click="viewPassword"><i class="far fa-eye"></i></div>

                <b-button
                        variant="primary" block
                        size="lg"
                        class="mt-3"
                        type="submit" @click.prevent @click="register">
                    <b-spinner small type="grow" v-if="isLoading"></b-spinner>
                    m'inscrire
                </b-button>
            </form>
        </div>
    </div>
    <div v-else>
        <div class="row">
            <div class="col-12 col-lg-6 offset-lg-3 pt-5 mt-5 row">
                <div class="col-lg-2">
                    <i class="fas fa-check fa-3x mb-3 text-success"></i>
                </div>
                <div class="col-lg-10">
                    <strong>C'est presque fini ! </strong><br/>

                    Il ne vous reste plus qu'à confirmer votre compte en cliquant sur le lien reçu par email.
                </div>
            </div>
        </div>
    </div>
</template>

<script>
  import {mapGetters} from 'vuex';

  export default {
    data() {
      return {
        passwordType: 'password',
        username: '',
        email: '',
        password: ''
      }
    },
    computed: {
      ...mapGetters('registration', [
        'isLoading',
        'isSuccess',
        'hasError',
        'errors'
      ])
    },
    methods: {
      viewPassword() {
        this.passwordType = this.passwordType === 'password' ? 'text' : 'password';
      },
      async register() {
        await this.$store.dispatch('registration/register', {
          username: this.username,
          email: this.email,
          password: this.password
        });
      }
    }
  }
</script>