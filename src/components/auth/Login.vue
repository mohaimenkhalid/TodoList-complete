<template>
	<div class="page-wrapper" >
		<div class="col-md-12 display-flex" >
			<div class="col-md-4"></div>
		<div class="col-md-4 ">
			
			<h2>Login</h2>
			<hr>
			<form @submit.prevent="login">

				<div v-if="successMessage" class="success-message">{{ successMessage }}</div>

				<div class="server-error" v-if="serverError" >
					{{ serverError }}
					<!-- {{ serverErrorEmptyField }} -->
				</div>
				  <div class="form-group">
				    <label for="exampleInputEmail1"><small>Email address</small></label>
				    <input name="email" type="email" class="form-control" id="exampleInputEmail1" aria-describedby="emailHelp" placeholder="Enter email" v-model="email">
				 
				  </div>
				  <div class="form-group">
				    <label for="exampleInputPassword1"><small>Password</small></label>
				    <input name="password" type="password" class="form-control" id="exampleInputPassword1" placeholder="Password" v-model="password"> 
				  </div>
				  <div class="form-group form-check"></div>

				  <button type="submit" class="btn btn-success btn-lg btn-block" :disabled="loading">
					  <div class="lds-ring-container" v-if="loading">
					  	<div class="lds-ring"><div></div><div></div><div></div><div></div></div>
					  </div>
				  Login
				</button>

			</form>
		</div>
		<div class="col-md-4"></div>
		</div>
		
	</div>
</template>

<script>
	
export default{
	
	data(){
		return{
			email: '',
			password: '',
			serverError:'',
			//serverErrorEmptyField:''
			successMessage: this.dataSuccessMessage,
			loading: false,
		}
	},
	props: {
		dataSuccessMessage:{
			type: String,

		}
	},

	methods:{
		login(){
			this.loading = true
			this.$store.dispatch('retriveToken', {
				email: this.email,
				password: this.password
			})

			.then(response => {
				this.loading = true
				this.$router.push({name : 'todo'});
			})
			.catch(error=>{
				this.loading = false
				//this.serverErrorEmptyField = Object.values(error.response.data.error_empty)
				this.serverError = error.response.data.error
				this.password=''
				this.succesMessage=''
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

</style>