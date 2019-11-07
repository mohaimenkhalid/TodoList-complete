<template>
	<div class="col-md-12 page-wrapper display-flex">
		<div class="col-md-4"></div>
		<div class="col-md-4 ">
			
			<h2>Register</h2>
			<hr>
			<form @submit.prevent="validateBeforeSubmit">

				<div class="server-error" v-if="serverError">
					<div v-for="serverError in serverError">
						{{ serverError[0] }}
					</div>
				</div>
				  <div class="form-group">
				  	 <label for="exampleInputEmail1"><small>Name</small></label>
				    <input name="name" type="name" class="form-control" id="exampleInputEmail1" aria-describedby="emailHelp" placeholder="Enter name" v-model="name" v-validate="'required'" :class="{'input-error':errors.has('name')}">
				    <span class="form-error">{{ errors.first('name') }}</span>
				</div>
				 <div class="form-group">
				    <label for="exampleInputEmail1"><small>Email address</small></label>
				    <input name="email" type="email" class="form-control" id="exampleInputEmail2" aria-describedby="emailHelp" placeholder="Enter email" v-model="email" v-validate="'required|email'" :class="{'input-error':errors.has('email')}">
				   <span class="form-error">{{ errors.first('email') }}</span>
				 
				  </div>
				  <div class="form-group">
				    <label for="exampleInputPassword1"><small>Password</small></label>
				    <input name="password" type="password" class="form-control" id="exampleInputPassword1" placeholder="Password" v-model="password" v-validate="'required|min:8'" ref="password" :class="{'input-error':errors.has('password')}"> 
				   <span class="form-error">{{ errors.first('password') }}</span>
				  </div>

				  <div class="form-group">
				    <label for="exampleInputPassword1"><small>Confirm Password</small></label>
				    <input name="confirm_password" type="password" class="form-control" id="exampleInputPassword2" placeholder="Confirm Password" v-model="confirm_password"  v-validate="'required|confirmed:password'" :class="{'input-error':errors.has('confirm_password')}"> 
				     <span class="form-error">{{ errors.first('confirm_password') }}</span>
				  </div>

				  <div class="form-group form-check"></div>
				  <button type="submit" class="btn btn-success">Create new account</button>

		
			</form>
		</div>

		<div class="col-md-4"></div>
		
	</div>
</template>

<script>
	
export default{
	
	data(){
		return{
			name: '',
			email: '',
			password: '',
			confirm_password:'',
			serverError:'',
			successMesage: '',
		}
	},

	methods:{

		 validateBeforeSubmit() {

      	this.$validator.validateAll().then((result) => {
        if (result) {
          // eslint-disable-next-line
          this.register();
        }
        //alert('Correct them errors!');
      });
    },


		register(){
			this.$store.dispatch('register', {
				name: this.name,
				email: this.email,
				password: this.password,
				confirm_password: this.confirm_password
			})

			.then(response => {

				this.successMesage = 'Registration Successfully';
				this.$router.push({name : 'login', params: {dataSuccessMessage: this.successMesage}});
			})
			.catch(error=>{
				this.serverError = Object.values(error.response.data.error)
			})
		}
	},

	
}
</script>


<style>

	.server-error{
		margin-bottom: 12px;
		font-size: 16px;
		padding: 10px 16px;
		color: #a94442;
		background: #F3DEDE;
		border-radius:4px;
	}

	.form-error{
		font-size: 16px;
		color: #a94442;
	}

	.input-error{
		border: 1px solid red;
	}

	.success-message{
		background-color: #dff0d8;
		color: #3c763d;
		margin-bottom: 12px;
		font-size: 16px;
		padding: 10px 16px;
		border-radius: 4px;
	}
	

</style>