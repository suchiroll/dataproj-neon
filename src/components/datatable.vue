<template>

<div id = "wrapper">
   <table>
      <th class = "name-header" @click="sort('first_name')">
      	Name 
      	<i v-show = "currentSortDir == 'asc' && currentSort == 'first_name'" class="material-icons">arrow_upward</i> 
      	<i  v-show = "currentSortDir == 'desc' && currentSort == 'first_name'" class="material-icons"> arrow_downward </i>
      </th>
      <th @click="sort('email')"> 
      	Email 
      	<i  v-show = "currentSortDir == 'asc' && currentSort == 'email'" class="material-icons">arrow_upward</i> 
      	<i  v-show = "currentSortDir == 'desc' && currentSort == 'email'" class="material-icons"> arrow_downward </i>
      </th>
      <th @click="sort('company')">
       Company 
       <i v-show = "currentSortDir == 'asc' && currentSort == 'company'" class="material-icons">arrow_upward</i> 
       <i v-show = "currentSortDir == 'desc' && currentSort == 'company'" class="material-icons"> arrow_downward </i>
   	   </th>
    <tr v-for="person of sortedPeople">
      <td class = "name"> {{person.first_name}} {{person.last_name}} </td>
      <td> {{person.email}} </td>
      <td> {{person.company}} </td>
    </tr>
  </table>
  <div id = "footer">
  	<div id = "items-control"> <b> Items per page: <select v-model = "pageSize" > 
 		<option> 5 </option>
  		<option> 10 </option>
  		<option> 20 </option>

  	</select></b> 
  		
  	 <b>  {{10*(currentPage-1) +1 }} - {{pageSize*currentPage}} of {{people.length}} </b> </div>
	 <div id = "page-control"> <b> {{currentPage}} of {{people.length/pageSize}} pages  </b> <button id ="prevpage-btn" @click = "prevPage"> < </button> <div id = "currentPage"> 
	 	<select v-model = "currentPage">
	 		<option v-for="index in people.length/pageSize"> {{index}} </option>
	 	</select>
	  </div>
	 	 <button id = "nextpage-btn" @click = "nextPage"> > </button> </div>
	 
  		
	</div>
</div>

</template>

<script>

import axios from 'axios';

export default {
  name: 'datatable',
 
  data(){
    return{
      people: [],
      currentSort: 'first_name',
      currentSortDir: 'asc',
      pageSize: 10,
      currentPage: 1

    }
  },
 
  created() {
    axios.get('https://staging.neonwebhosting.com/mockdata/people.json')
    .then(response => {
         this.people = response.data
    })
    .catch(error => {
      console.log(error);
    })
    }, methods:{
   	sort:function(filter) {
      //if filter == current sort, reverse
      if(filter === this.currentSort) {
        this.currentSortDir = this.currentSortDir==='asc'?'desc':'asc';
      }
      this.currentSort = filter;
    },
    nextPage:function() { //simple function to go to next page
      if((this.currentPage*this.pageSize) < this.people.length) this.currentPage++;
    },
    prevPage:function() { //simple function to go to previous page
      if(this.currentPage > 1) this.currentPage--;
   	 },

    }, 
    computed:{
    sortedPeople:function() { //sort by current sorting direction
   	   return this.people.sort((a,b) => {
   	  
        let modifier = 1;
        if(this.currentSortDir === 'desc') modifier = -1;

        if(a[this.currentSort] == null){
        	a[this.currentSort] = "";
        } else if (b[this.currentSort] == null){
        	b[this.currentSort] = "";
        }

        if(a[this.currentSort] < b[this.currentSort]){ 
        	
        	return -1 * modifier;
        }

        if(a[this.currentSort] > b[this.currentSort]) {
        		
        	return 1 * modifier; 
        }
        return 0;
      }).filter((row, index) => { //limit view and not repeat due to pagination
        let start = (this.currentPage-1)*this.pageSize;
        let end = this.currentPage*this.pageSize;
        if(index >= start && index < end) return true;
      });
    }
    }
}
</script>

<style>

@import url('https://fonts.googleapis.com/css?family=Lato|Roboto');


	body, html{
		
		width, height: 100%;
		text-align: left;
	
	}
	body{
		background-color: #F7F9FA;
	}
	#wrapper{
		background-color: #fff;
	font-family: 'Lato', serif;
		width: 900px;
		margin-left: auto;
		margin-right: auto;
		box-shadow:0px 0px 5px #C8C9CA;
	}
	table{
		font-size: 14px;
		
		border-collapse: collapse;
		
		width: 900px;

	}

	.name-header, .name{
		padding-left: 30px;
		max-width: 160px;
		min-width: 160px;
	}
	th{
	background-color: #fff;
		padding-left: 60px;
		padding-top: 30px;
		height: 32px;
		text-align: left;
		border-bottom: 2px solid  #E1E7EE ;
		border-top: 1px solid  #E1E7EE ;
	}

	th:hover{
		background-color: #F4F6F9;
	}
	tr{
		padding-left: 25px;
		text-align: left;
		height: 60px;
		background-color: #fff;

	}

	td{
		max-width: 200px;
		min-width: 200px;
		padding-left: 60px;
		padding-right: 60px;
		margin-left: 0px;
		border-bottom: 1px solid  #E1E7EE ;
	}

	tr:hover{
		background-color: #DAEDFE;
		border-bottom: 2px solid  #E1E7EE ;
	}


	.name{
		color: #0074D9;
	}

	#footer{
		width: 900px;
		font-size: 12px;
		height: 35px;
		
	}

	button{

		color: #0074D9;
		margin: 0px;
		padding: 0px;
		height: 35px;
		width: 40px;
		border-right: 1px solid  #E1E7EE;
		border-top: none;
		border-bottom: none;
		border-left: 1px solid  #E1E7EE;
		background-color: #fff;
	}

	button:hover{
		background-color: #DAEDFE;
	}
	select{
		color: #0074D9;
		border:none;
		text-align: center;
	}

	option{
		font-family: 'Roboto', sans-serif;
		border: none;
		text-align: center;
	}

	#items-control, #page-control, #currentPage{
		display: inline-block;
		height: 35px;
	}

	#items-control{
		padding-top: 10px;
		padding-left: 30px;
	}

	#page-control{

		float: right;
		width: 600px;
		text-align: right;
	}

	#prevpage-btn{
		margin-left: 10px;
	}

	#nextpage-btn{
		border-right:none;
	}

	.material-icons{
		font-size: 12px;
	}
	
</style> 