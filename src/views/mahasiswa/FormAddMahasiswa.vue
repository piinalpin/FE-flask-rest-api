<template>
	<div class="animated fade-id">
		<b-row>
			<b-col md="12">
				<b-card>
					<div slot="header">
						<strong>Add Student </strong><small>Form</small>
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
				body: {
					name: '',
					nim: ''
				}
			}
		},
    methods: {
        showConfirm(){
            this.$swal.fire({
						  title: 'Are you sure?',
						  text: "You won't be able to revert this!",
						  type: 'warning',
						  showCancelButton: true,
						  confirmButtonColor: '#4dbd74',
						  cancelButtonColor: '#f64846',
						  confirmButtonText: 'Yes, save it!',
						  reverseButtons: true,
						}).then((result) => {
							axios.post(process.env.VUE_APP_ROOT_API + '/mahasiswa', {
									body: this.body
								}).then(response => console.log(response)).catch(e => this.errors.push(e));
						  if (result.value) {
						    this.$swal.fire({
						    	type: 'success',
						    	title: 'Success!',
						    	text: 'Data has been saved!',
						    	confirmButtonColor: '#4dbd74'
						    }).then(this.$router.push('/mahasiswa/list'));
						  }
						})
        }
    }
}
</script> 