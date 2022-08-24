<template>
  <section class="bg-info vh-100" @submit.prevent="login">
    <div class="container py-5 h-100">
      <div class="row d-flex justify-content-center align-items-center h-100">
        <div class="col col-xl-10">
          <div class="card" style="border-radius: 1rem">
            <div class="row g-0">
              <div class="col-md-6 col-lg-5 d-none d-md-block">
                <img src="../images/Meeting-app.png" alt="login form" class="img-fluid"
                  style="border-radius: 1rem 0 0 1rem" />
              </div>
              <div class="col-md-6 col-lg-7 d-flex align-items-center">
                <div class="card-body p-4 p-lg-5 text-black">
                  <form>
                    <div class="d-flex align-items-center mb-3 pb-1">
                      <i class="fas fa-cubes fa-2x me-3" style="color: #ff6219"></i>
                    </div>

                    <h5 class="mb-3 pb-3">
                      Sign into your account


                    </h5>

                    <div class="form-outline mb-4">
                      <input v-model="email" type="email" class="form-control form-control-lg" id="form2Example17"
                        required />
                      <label class="form-label error" for="form2Example17">Email address</label>
                    </div>

                    <div class="form-outline mb-4">
                      <input v-model="password" type="password" id="form2Example27" class="form-control form-control-lg"
                        required />
                      <label class="form-label" for="form2Example27">Password</label>
                    </div>

                    <div class="pt-1 mb-4">
                      <button @click="onsubmit()" class="btn btn-danger btn-lg btn-block">
                        Login
                      </button>
                    </div>

                    <p class="mb-5 pb-lg-2">
                      Don't have an account?
                      <a href="/register">Register here</a>
                    </p>


                    <span class="alert alert-danger text-center" v-show="wrongEmail"
                      style="color: red; margin-left: 40px">Email is not
                      correct !!</span>
                    <span v-if="wrongEmail == false" class="alert alert-danger text-center" v-show="errorMessage"
                      style="color: red; margin-left: 40px">Credentials not matched !!</span>
                  </form>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>


import { Login } from '@/services/loginPage'
export default {
  data() {
    return {
      email: "",
      password: "",
      wrongEmail: "",
      errorMessage: "",
    };
  },

  methods: {
    onsubmit() {
      if (
        /^(([^<>()[\].,;:\s@"]+(\.[^<>()\\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,24}))$/.test(
          this.email
        )
      ) {
        this.wrongEmail = false;
      } else {
        this.wrongEmail = true;
      }

    },

    async login() {
      const credentials = {
        email: this.email,
        password: this.password,
      };
      Login(credentials)
        .then(response => {
          const data = response.data;
          localStorage.setItem("token", data.token);
          localStorage.setItem("email", data.email);
          localStorage.setItem("name", data.name);
          this.$router.push({ path: "/meetings/add" });
        })
        .catch(() => {
          if (!this.wrongEmail) {
            this.errorMessage = true;
          }
        })



    },
  },
};
</script>

<style scoped>
</style>