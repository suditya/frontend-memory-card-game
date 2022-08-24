<template>
    <section>
        <NavBar />

        
        <div class="container p-5">
            <h1 class="mb-3 pb-3 text-center">Add an account</h1>
            <div class="row d-flex justify-content-center align-items-center">
                <div class="col col-xl-10">
                    <div class="card rounded bg-warning">
                        <div class="row g-0">
                            <div class="col-md-6 col-lg-5 d-none d-md-block bg-warning rounded">
                                <img src="../images/email.png" alt="login form" class="img-fluid" style="
                    border-radius: 1rem 0 0 1rem;
                    margin-top: 150px;
                    margin-bottom: 100px;
                  " />
                            </div>
                            <div class="col-md-6 col-lg-7 d-flex align-items-center">
                                <div class="card-body bg-warning rounded">
                                    <form @submit.prevent="submit">

                                        <div class="form-outline">
                                            <label class="form-label" for="form-control">Company Name</label>
                                            <input type="text" class="form-control" v-model.trim=companyName
                                                name="name" />

                                        </div>

                                        <div class="form-outline">
                                            <label class="form-label" for="form-control">Email address</label>
                                            <input type="email" class="form-control" v-model.trim="$v.email.$model"
                                                :class="{
                                                    'is-invalid': $v.email.$error,
                                                    'is-valid': !$v.email.$invalid,
                                                }" name="email" />
                                            <div class="valid-feedback">Your email is valid</div>
                                            <div class="invalid-feedback">
                                                <span v-if="!$v.email.required">email is required</span>
                                                <span v-if="!$v.email.isUnique">This email is wrong</span>
                                            </div>
                                        </div>

                                        <div class="form-outline">
                                            <label class="form-label" for="form-control">Password</label>
                                            <input type="password" class="form-control"
                                                v-model.trim="$v.password.$model" :class="{
                                                    'is-invalid': $v.password.$error,
                                                    'is-valid': !$v.password.$invalid,
                                                }" name="password" />

                                            <div class="valid-feedback">Your password is valid</div>
                                            <div class="invalid-feedback">
                                                <span v-if="!$v.password.required">password is required</span>
                                                <span v-if="!$v.email.validatePassword">Password should contains minimum
                                                    8 characters and
                                                    atleast 1 uppercase, lowercase, and special
                                                    character</span>
                                            </div>
                                        </div>

                                        <div class="mt-3">
                                            <button class="btn btn-danger btn-lg btn-block" :disabled="$v.$invalid">
                                                Add
                                            </button>
                                        </div>



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
import { required, minLength, email } from 'vuelidate/lib/validators'
import NavBar from './NavBar.vue';
import { AddAccount } from '@/services/addAccount'
export default {
    name: "AddAccount",
    components: { NavBar },
    data() {
        return {
            cards: [
                {
                    name: '',
                    email: ''
                },
                {
                    name: '',
                    email: ''
                },
                {
                    name: '',
                    email: ''
                }
            ],
            email: '',
            password: '',
            companyName: '',
            userId: '',

        }
    },
    
    methods: {
        async submit() {
            this.$v.$touch();
            console.log('touched')
            const credentials = {
                userId: localStorage.getItem("userId"),
                companyName: this.companyName,
                email: this.email,
                password: this.password,
            }
            console.log(credentials)
            AddAccount(credentials).then((result) => {
                console.log("result is: ", result.data)
                if (result.data.status === "FAILED") {
                    this.$toasted.show(result.data.message);
                } else {
                    this.$toasted.show(result.data.message);
                    this.$router.push({ name: 'mailAccounts' })
                }
            })
        }
    },
}
</script>

<style>
</style>

    <!-- data()
    {
        return 
        {
            cards:[
                {
                    imagename: 'Apple',
                    img: 'apple.gif',

                },
                {
                    imagename: 'Banana',
                    img: 'banana.gif',
 
                },
                {
                    imagename: 'Orange',
                    img: 'orange.jpg',

                },
                {
                    imagename: 'Pineapple',
                    img: 'pineapple.png',

                },
                {
                    imagename: 'Strawberry',
                    img: 'strawberry.png',

                },
                {
                    imagename: 'watermelon',
                    img: 'watermelon.jpg',

                },
            ]
        }
    }, -->