<template>
  <div class="animated fadeIn">
  	<notifications group="errorMessage" position="top right" animation-type="velocity" style="top:58px;"/>
    <b-row>
      <b-col lg="12" v-if="show">
      	<b-card :header="captions">
      		<b-table :fields="fields" :items="items" ref="table">
      			<template slot="name" slot-scope="data">
      				{{ data.item.name }}
      			</template>
      			<template slot="nim" slot-scope="data">
      				{{ data.item.nim }}
      			</template>
      			<template slot="actions" slot-scope="data">
      				<b-button @click="goEditForm(data.item.id)" class="btn btn-warning"><i class="fa fa-pencil"></i></b-button>&nbsp
      				<b-button @click="deleteMahasiswa(data.item.id)" class="btn btn-danger"><i class="fa fa-trash"></i></b-button>
      			</template>
      		</b-table>
      		<nav>
		      <b-pagination :total-rows="totalRows" :per-page="perPage" v-model="currentPage" prev-text="Prev" next-text="Next" hide-goto-end-buttons/>
		    </nav>
      	</b-card>
      </b-col>
    </b-row>
    <!--/.row-->
  </div>

 	<!-- <b-table :fields="fields" :items="items">
 		<template slot="test" slot-scope="data">
 			<button class="btn btn-primary">edit</button>
 		</template>
 	</b-table> -->

</template>

<script>
	import cTable from '@/views/base/Table.vue';
	import axios from 'axios';

	export default {
	  name: 'mahasiswa',
	  components: {cTable},
	  data: () => {
	    return {
	      items:[],
	      fields: [
	        {key: 'name', label: 'Name'},
	        {key: 'nim', label: 'Identity Number'},
	        {key: 'actions', label: 'Actions'}
	      ],
	      show: true,
	      currentPage: 1,
	      perPage: 10,
	      endpoint: ''
	    }
	  },
	  computed: {
	    totalRows: function () { return this.getRowCount() },
	    captions: function() { return "<i class='fa fa-align-justify'></i> Daftar Mahasiswa" }
	  },
	  created() {
	  	this.endpoint = this.$router.history.current.path;
	  	console.log(this.endpoint);
	  },
	  methods: {
	  	rerender() {
	  		this.show = false;
	  		this.$nextTick(() => {
	  			this.show = true;
	  			console.log('Re render start');
	  			axios.get(process.env.VUE_APP_ROOT_API + this.endpoint).then((res) => {
			  		this.items = res.data.mahasiswa;
			  		console.log(this.items);
			  	}).catch((error) => {
			  		if (!error.res) {
			  			this.$notify({
			  				group: 'errorMessage',
			  				type: 'error',
			  				text: 'ERR_CONNECTION_REFUSED',
			  				duration: 5000,
			  			});
			  		} else {
			  			console.log(error.res.data.error);
			  		}
			  	});
	  			this.$nextTick(() => {
	  				console.log('Re render end');
	  			})
	  		})
	  	},
	  	getRowCount: function () {
	      return this.items.length
	    },
	    goEditForm(id) {
	    	return this.$router.push({name: 'Edit Mahasiswa', params: {id: id}})
	    },
	    deleteMahasiswa(id) {
	    	const deleteEndPoint = this.endpoint + '/' + id;
	    	this.$swal.fire({
			  title: 'Are you sure?',
			  text: "You won't be able to revert this!",
			  type: 'warning',
			  showCancelButton: true,
			  cancelButtonColor: '#d33',
			  confirmButtonText: 'Yes, save it!',
			  reverseButtons: true,
			}).then((result) => {
				axios.delete(process.env.VUE_APP_ROOT_API + deleteEndPoint).then(response => console.log(response)).catch(e => this.errors.push(e));
			  if (result.value) {
			    this.$swal.fire({
			    	type: 'success',
			    	title: 'Success!',
			    	text: 'Data has been deleted!',
			    	confirmButtonColor: '#4dbd74'
			    }).then((update) => {
			    		axios.get(process.env.VUE_APP_ROOT_API + this.endpoint).then((res) => {
					  		this.items = res.data.mahasiswa;
					  		console.log(this.items);
					  	});
					  	this.rerender();
			    });
			  }
			})
	    }
	  },
	  beforeMount() {
	  	axios.get(process.env.VUE_APP_ROOT_API + this.endpoint).then((res) => {
		  		this.items = res.data.mahasiswa;
		  		console.log("Before Mount");
		  		this.rerender();
		  	}).catch((error) => {
		  		if (!error.res) {
		  			console.log("ERR_CONNECTION_REFUSED");
		  		} else {
		  			console.log(error.response.data.error);
		  		}
		  	});
	  }
	}
</script>