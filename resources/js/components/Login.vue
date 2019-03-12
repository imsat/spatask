<template>
    <div class="modal fade" tabindex="-1" role="dialog" id="loginModal">
        <div class="modal-dialog" role="document">


            <div class="modal-content">
                <form>
                <div class="modal-header">
                    <h4 class="modal-title text-center">Login</h4>
                </div>
                <ul class="alert alert-danger" v-if="errors.length > 0">
                    <p class="text-center" v-for="error in errors" :key="errors.indexOf(error)">{{error}}</p>
                </ul>
                <div class="modal-body">
                    <div class="form-group">
                        <input type="text" class="form-control" placeholder="Email" v-model="email">
                    </div>
                    <div class="form-group">
                        <input type="password" class="form-control" placeholder="Password" v-model="password">
                    </div>
                    <div class="form-group">
                        <label>
                            <input type="checkbox" v-model="remember">
                            <span class="">Remember me</span>
                        </label>

                    </div>
                </div>
                <div class="modal-footer">
                    <button class="btn btn-bold btn-primary" @click.prevent="attemtLogin()" :disabled="!isValidLoginForm" type="submit">Login</button>
                    <a class="text-muted hover-primary fs-13" href="#">Forgot Your password?</a>
                </div>
                </form>
            </div><!-- /.modal-content -->

        </div><!-- /.modal-dialog -->
    </div><!-- /.modal -->
</template>

<script>
    import axios from 'axios'
    export default {
        data(){
            return {
                email: '',
                password: '',
                remember: true,
                loading: false,
                errors: [],
            }
        },
        computed: {
            isValidLoginForm() {
                return this.emailIsValid() && this.password && !this.loading
            }
        },
        methods: {
            emailIsValid() {
                if (/^\w+([\.-]?\w+)*@\w+([\.-]?\w+)*(\.\w{2,3})+$/.test(this.email)) {
                    return true;
                }else{
                    return false;
                }
            },
            attemtLogin(){
                this.errors = [];
                this.loading = true;
                axios.post('/login',{
                    email: this.email, password: this.password, remember: this.remember
                }).then(resp => {
                    location.reload();
                }).catch(error =>{
                    this.loading = false;
                    if(error.response.status == 422){
                        this.errors.push("We couldn't verify your account details.")
                    }else{
                        this.errors.push("Somethinh went wrong, please refresh and try again.")
                    }
                })
            }
        }
    }
</script>

<style scoped>

</style>
