                                  
<script>

  import router from '../router';
  import {APIService} from '../http/APIService';
  const apiService = new APIService();

  export default {
    name: 'Register',

    data: () => ({
      credentials: {},
      password: "",
      repassword: "",
      valid: true,
      showMsg: '',
      loading: false,
      rules: {
        username: [
          v => !!v || "Username is required",
          v => (v && v.length > 3) || "A username must be more than 3 characters long",
          v => /^[a-z0-9_]+$/.test(v) || "A username can only contain letters and digits"
        ],
        password: [
          v => !!v || "Password is required",
          v => (v && v.length > 7) || "The password must be longer than 7 characters"
        ],
        email: [
          v => !!v || "Email is required"
        ],
        repassword: [
          v => (v == this.password) || 'Passwords must match'
        ]
      },
      showPassword: false,
    }),
    methods: {
      register() {
        
       apiService.registerUser(this.credentials).then(response => {
          if (response.status === 201) {
            this.movie = response.data;
            this.showMsg = "";
            router.push('/auth/');
          }else{
            this.showMsg = "error";
          }
        }).catch(error => {
          if (error.response.status === 401) {
            router.push("/auth");
          }else if (error.response.status === 400) {
            this.showMsg = "error";
          }
        });
      },
    },
    computed: {
      passwordConfirmationRule() {
        return (this.password === this.repassword) || 'Password must match'
    }
    }
  }
</script>
