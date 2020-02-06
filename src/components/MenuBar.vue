<template>
    <div>
        <div class="menu-bar navbar">
            <button v-if="!login" @click="() => inUp = true" type="button" class="btn btn-dark" data-toggle="modal" data-target="#exampleModalCenter">Login</button>
            <button v-if="!login" @click="() => inUp = false" type="button" class="btn btn-dark" data-toggle="modal" data-target="#exampleModalCenter">Sign up</button>
            <button v-if="login" type="button" class="btn btn-dark" @click="() => onLogOut()">LogOut</button>
        </div>

        <div class="modal fade" id="exampleModalCenter" tabindex="-1" role="dialog" aria-labelledby="exampleModalCenterTitle" aria-hidden="true">
            <div class="modal-dialog modal-dialog-centered" role="document">
                <div v-if="inUp" class="modal-content">
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
                                    @keydown.space.prevent=""
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
                <div v-if="!inUp" class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="exampleModalCenterTitle">Registration</h5>
                        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                        <span aria-hidden="true">&times;</span>
                        </button>
                    </div>
                    <div class="modal-body">
                        <form @submit.prevent="authSignUp">
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
                                    @keydown.space.prevent=""
                                    >
                                <label for="inputPasswordConf" class="label-conf">Confirm password</label>
                                <input 
                                    type="password" 
                                    class="form-control" 
                                    id="inputPasswordConf" 
                                    @blur="isValid"
                                    @keydown.space.prevent=""
                                    >
                            </div>
                            <!-- <div class="form-group form-check">
                                <input type="checkbox" class="form-check-input" id="exampleCheck">
                                <label class="form-check-label" for="exampleCheck">Check me out</label>
                            </div> -->
                            <button id="inputSubmit" type="submit" class="btn btn-primary">Sign in</button>
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
           },
           login: false,
           inUp: true,
           showMenu: true,

           startPosScroll: 0,
        }
    },
    watch: {
        "inValidator.emailInp": function (v) {
            if (v) {
                document.getElementById("inputEmail").classList.add("no-valid");
            } else {
                document.getElementById("inputEmail").classList.remove("no-valid");
            }
        },

    },
    methods: {
        handlerHiddenMenuBar() {  // скрытие и появление навигации при скроллинге
        //console.log(this.showMenu, this.startPosScroll, window.pageYOffset)
            if (window.pageYOffset > this.startPosScroll) {               
                if (this.showMenu == true) {              
                    this.startPosScroll = window.pageYOffset;                    
                    document.body.querySelector(".menu-bar").classList.add("menu-bar-hiddenUp");
                    this.showMenu = false;
                } 
                              
            } else {
                if (this.showMenu == false || window.pageYOffset == 0) {
                    this.startPosScroll = window.pageYOffset;
                    document.body.querySelector(".menu-bar").classList.remove("menu-bar-hiddenUp");
                    this.showMenu = true;
                }
            }
        },
        authSignUp(event) {
            const email = event.target.querySelector("#inputEmail").value;
            const password = event.target.querySelector("#inputPassword").value;
            const passwordConfirm = event.target.querySelector("#inputPasswordConf").value;

            console.log(email);


            if (passwordConfirm != password) {
                event.target.querySelector("#inputPasswordConf").classList.add("is-invalid");

                return 0;
            } else {
                event.target.querySelector("#inputEmail").classList.add("is-valid");
                event.target.querySelector("#inputPassword").classList.add("is-valid");
                if (event.target.querySelector("#inputPasswordConf").classList.contains("is-invalid")) {
                    event.target.querySelector("#inputPasswordConf").classList.remove("is-invalid");
                }
                event.target.querySelector("#inputPasswordConf").classList.add("is-valid");
            }
            
            const apiKey = 'AIzaSyDTCjDnPrZ8Rdj5XAAWVrlxzvM0yCn_XOI';
                fetch(`https://identitytoolkit.googleapis.com/v1/accounts:signUp?key=${apiKey}`, {
                    method: "POST",
                    body: JSON.stringify({
                        email, password,
                        returnSecureToken: true
                    }),
                    headers: {
                        "Content-Type": "application/json"
                    }
                })
                    .then(response => response.json)
                    .then(() => this.authWEmailAPassord(email, password))
                    .then(token => {
                        let date = new Date(Date.now() + 86400e3);
                        
                        this.setCookie("idToken", token, {expires: date});
                    })
                    .then(() => window.location.reload());

        },
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
                .then(token => {
                        let date = new Date(Date.now() + 86400e3);
                        //document.cookie =  `idToken=${encodeURIComponent(token)}; path=/; expires=${date}`
                        
                        this.setCookie("idToken", token, {expires: date});
                    })
                .then(() => window.location.reload());
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
                .then(data => data.idToken)
                
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
        },
        onLogOut() {
            this.deleteCookie('idToken');
            location.reload();
        },
        getCookie(name) {
            let matches = document.cookie.match(new RegExp(
                "(?:^|; )" + name.replace(/([\.$?*|{}\(\)\[\]\\\/\+^])/g, '\\$1') + "=([^;]*)"
            ));
            return matches ? decodeURIComponent(matches[1]) : undefined;
        },
        setCookie(name, value, options = {}) {
            options = {
                path: '/',

                ...options
            };

            if (options.expires.toUTCString) {
                options.expires = options.expires.toUTCString();
            }

            let updatedCookie = encodeURIComponent(name) + "=" + encodeURIComponent(value);

            for (let optionKey in options) {
                updatedCookie += "; " + optionKey;
                let optionValue = options[optionKey];
                if (optionValue !== true) {
                updatedCookie += "=" + optionValue;
                }
            }

            document.cookie = updatedCookie;
        },
        deleteCookie(name) {
            this.setCookie(name, "", {
                path: '/',
                expires: 0,
                'max-age': -1,
            })
        }        
    },
    mounted() {
        if (this.getCookie("idToken")) {
            this.login = true;
        }

        window.addEventListener("scroll", this.handlerHiddenMenuBar); // создания слушателя скроллинга для скрытия\появления навигации
    }    
}

</script>

<style scoped>


.menu-bar {
    z-index: 1;
    background-color: rgb(89, 173, 127);
    position: fixed;
    top: 0;
    left: 0;
    height: 60px;
    width: 100%;
    opacity: 100%;
    transition-property: top, opacity;
    transition-duration: .3s;
}

.menu-bar-hiddenUp {
    top: -60px;
    opacity: 0%;
}

.no-valid {
    background-color: rgba(250, 6, 6, 0.192);
}

.label-conf {
   margin-top: 14px;
}

</style>
