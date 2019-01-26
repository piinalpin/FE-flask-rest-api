<template>
	<div class="animated fade-id">
		<notifications group="errorMessage" position="top right" animation-type="velocity" style="top:58px;"/>
		<b-row>
			<b-col md="12">
				<b-card>
					<div slot="header">
						<strong>Edit Student </strong><small>Form</small>
						<b-button v-on:click="showConfirm" class="float-right" size="sm" variant="primary"><i class="fa fa-dot-circle-o"></i> Submit</b-button>
					</div>
					<b-form-group>
	          <label for="company">Name</label>
	          <b-form-input type="text" name="name" v-model="body['name']" id="name"></b-form-input>
	        </b-form-group>
	        <b-form-group>
	          <label for="vat">Idenity Number</label>
	          <b-form-input type="text" name="nim" v-model="body['nim']" id="nim"></b-form-input>
	        </b-form-group>
	        <!-- <b-row>
	          <b-col sm="8">
	            <b-form-group>
	              <label for="city">City</label>
	              <b-form-input type="text" id="city" placeholder="Enter your city"></b-form-input>
	            </b-form-group>
	          </b-col>
	          <b-col sm="4">
	            <b-form-group>
	              <label for="postal-code">Postal Code</label>
	              <b-form-input type="text" id="postal-code" placeholder="Postal Code"></b-form-input>
	            </b-form-group>
	          </b-col>
	        </b-row> -->
				</b-card>
			</b-col>
		</b-row>
	</div>
</template>
<script>
import axios from 'axios'

export default {
	data: () => {
		return {
			body: {},
			endpoint: ''
		}
	},
	mounted() {
		this.endpoint = this.$router.history.current.path;
		axios.get(process.env.VUE_APP_ROOT_API + this.endpoint).then((res) => {
			  		this.body = res.data.mahasiswa;
			  		console.log(this.body);
			  	}).catch((error) => {
			  		console.log(error.response.statusText);
			  		if (error.response.statusText == 'NOT FOUND') {
			  			this.$notify({
			  				group: 'errorMessage',
			  				type: 'error',
			  				text: 'Data mahasiswa: not found',
			  				duration: 5000,
			  			});
			  		} else {
			  			this.$notify({
			  				group: 'errorMessage',
			  				type: 'error',
			  				text: 'ERR_CONNECTION_REFUSED',
			  				duration: 5000,
			  			});
			  		}
			  	});
		console.log(this.$router.history.current.path);
	},
    methods: {
        showConfirm(){
            this.$swal.fire({
			  title: 'Are you sure?',
			  text: "You won't be able to revert this!",
			  type: 'warning',
			  showCancelButton: true,
			  cancelButtonColor: '#d33',
			  confirmButtonText: 'Yes, save it!',
			  reverseButtons: true,
			}).then((result) => {
				axios.put(process.env.VUE_APP_ROOT_API + this.endpoint, {
						body: this.body
					}).then(response => console.log(response)).catch(e => this.errors.push(e));
			  if (result.value) {
			    this.$swal.fire({
			    	type: 'success',
			    	title: 'Success!',
			    	text: 'Data has been edited!',
			    	confirmButtonColor: '#4dbd74'
			    }).then(this.$router.push('/mahasiswa'));
			  }
			})
        }
    }
}
</script> 