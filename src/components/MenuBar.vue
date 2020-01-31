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
                                <input type="email" class="form-control" id="inputEmail" aria-describedby="emailHelp">
                                <!-- <small id="emailHelp" class="form-text text-muted">We'll never share your email with anyone else.</small> -->
                            </div>
                            <div class="form-group">
                                <label for="inputPassword">Password</label>
                                <input type="password" class="form-control" id="inputPassword">
                            </div>
                            <!-- <div class="form-group form-check">
                                <input type="checkbox" class="form-check-input" id="exampleCheck">
                                <label class="form-check-label" for="exampleCheck">Check me out</label>
                            </div> -->
                            <button type="submit" class="btn btn-primary">Login</button>
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
           
        }
    },
    methods: {
        authFormHandler(event) {
            
            const email = event.target.querySelector("#inputEmail").value;
            const password = event.target.querySelector("#inputPassword").value;

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
        }
    }
    
}
</script>

<style scoped>

.menu-bar {
    position: fixed;
    top: 0;
    left: 0;
    height: 100px;
    width: 100%;
}

</style>