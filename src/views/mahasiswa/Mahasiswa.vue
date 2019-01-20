<template>
  <div class="animated fadeIn">
  	<notifications group="errorMessage" position="top right" animation-type="velocity" style="top:58px;"/>
    <b-row>
      <b-col lg="12" v-if="show">
        <c-table :table-data="items" :fields="fields" caption="<i class='fa fa-align-justify'></i> Daftar Mahasiswa"></c-table>
      </b-col>
    </b-row><!--/.row-->
  </div>

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
	        {key: 'nim', label: 'Identity Number'}
	      ],
	      show: true
	    }
	  },
	  methods: {
	  	rerender() {
	  		this.show = false;
	  		this.$nextTick(() => {
	  			this.show = true;
	  			console.log('Re render start');
	  			axios.get(process.env.VUE_APP_ROOT_API + '/mahasiswa').then((res) => {
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
	  	}
	  },
	  beforeMount() {
	  	axios.get(process.env.VUE_APP_ROOT_API + '/mahasiswa').then((res) => {
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
	  	this.rerender();
	  }
	}
</script>