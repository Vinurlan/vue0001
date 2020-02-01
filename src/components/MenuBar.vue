<template>
    <div class="">
        <div class="menu-bar navbar">
            <button type="button" class="btn btn-dark" data-toggle="modal" data-target="#exampleModalCenter">Login</button>
        </div>

        <div class="modal fade" id="exampleModalCenter" tabindex="-1" role="dialog" aria-labelledby="exampleModalCenterTitle" aria-hidden="true">
            <div class="modal-dialog modal-dialog-centered" role="document">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="exampleModalCenterTitle">Authentication</h5>
                        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                        <span aria-hidden="true">&times;</span>
                        </button>
                    </div>
                    <div class="modal-body">
                        <form @submit.prevent="authFormHandler">
                            <div class="form-group">
                                <label for="inputEmail">Email address</label>
                                <input type="email" class="form-control" id="inputEmail" aria-describedby="emailHelp" @blur="isValid">
                                <small :is="!isValid" id="emailHelp" class="form-text text-muted">We'll never share your email with anyone else.</small>
                            </div>
                            <div class="form-group">
                                <label for="inputPassword">Password</label>
                                <input 
                                    type="password" 
                                    class="form-control" 
                                    id="inputPassword" 
                                    @blur="isValid"
                                    @keyup.space.prevent=""
                                    >
                            </div>
                            <!-- <div class="form-group form-check">
                                <input type="checkbox" class="form-check-input" id="exampleCheck">
                                <label class="form-check-label" for="exampleCheck">Check me out</label>
                            </div> -->
                            <button id="inputSubmit" type="submit" class="btn btn-primary">Login</button>
                        </form>
                    </div>
                    <div class="modal-footer">
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "MenuBar",
    data() {
        return {
           inValidator: {
               emailInp: false,
               passwordInp: false,
           }
        }
    },
    watch: {
        "inValidator.emailInp": function (v) {
            if (v) {
                document.getElementById("inputEmail").classList.add("no-valid");
            } else {
                document.getElementById("inputEmail").classList.remove("no-valid");
            }
        }
    },
    methods: {
        authFormHandler(event) {
            
            const email = event.target.querySelector("#inputEmail").value;
            const password = event.target.querySelector("#inputPassword").value;

            if (email.trim() == "" || password.trim() == "") {
                let targetSub = event.target.querySelector("#inputSubmit");

                targetSub.disabled = true;
                targetSub.classList.add("no-valid");
                setTimeout(() => {
                    targetSub.classList.remove("no-valid"); 
                    targetSub.disabled = false;
                    }, 1000);
                return;
            }

            this.authWEmailAPassord(email, password)
                // .then(token => {

                // })
        },
        authWEmailAPassord(email, password) {
            const apiKey = 'AIzaSyDTCjDnPrZ8Rdj5XAAWVrlxzvM0yCn_XOI';
            return fetch(`https://identitytoolkit.googleapis.com/v1/accounts:signInWithPassword?key=${apiKey}`, {
                method: "POST",
                body: JSON.stringify({
                    email, password,
                    returnSecureToken: true
                }),
                headers: {
                    "Content-Type": "application/json"
                }
            })
                .then(response => response.json())
                .then(data => data.idToken);
        },
        isValid(event) {
            const target = event.target;

            if (target.value.trim() == "") {
                this.inValidator.emailInp == true;
                //target.classList.add("no-valid");
            } else {
                if (target.classList.contains("no-valid")) {
                    this.inValidator.emailInp == false;
                    //target.classList.remove("no-valid")
                }
            }
        }
    }
    
}
</script>

<style scoped>

.menu-bar {
    background-color: rgb(89, 173, 127);
    position: fixed;
    top: 0;
    left: 0;
    height: 60px;
    width: 100%;
}

.no-valid {
    background-color: rgba(250, 6, 6, 0.192);
}

</style>