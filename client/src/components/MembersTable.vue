<template>
  <div class="membersTable">
		<div class="loader" v-if="loading"><CircularLoader :show="true"/></div>
		<h2>Members</h2>
		<table>
			<thead>
        <tr>
          <th>First Name</th>
          <th>Last Name</th>
          <th>Adress</th>
          <th>SSN</th>
        </tr>
      </thead>
			<tbody>
        <tr v-for="(member, index) in membersArray" :key="index">
					<td v-for="(data, index) in member" :key="index">{{data}}</td>
				</tr>
			</tbody>
		</table>
  </div>
</template>

<script>
import CircularLoader from './CircularLoader';
export default {
  name: 'MembersTable',
	components: {CircularLoader},
	// data() {
	// 	return {
	// 	}
	// },
	props:{
		membersArray: [],
		// eslint-disable-next-line vue/require-prop-type-constructor
		loading: false
	},
	}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
	.membersTable{
		position: relative;
		width: fit-content;
		min-width: 50%;
		padding: 20px 25px;
		background-color: #fff;
		box-shadow: 0px 0px 10px rgb(197, 197, 197);
		border-radius: 0.5rem;
	}
	.loader{
		top: 0;
		left: 0;
		position: absolute;
		width: 100%;
		height: 100%;
		background: rgba(45, 71, 110, 0.2);
		z-index: 10;
		text-align: center;
	}
	h2{
		margin-bottom: 10px;
		font-size: 1.2em;
	}
	table{
		position: relative;
		width: 100%;
		border-collapse: collapse;
		border-spacing: 0px 15px;
	}
	thead {
		text-align: left;
		border-bottom: solid 2px #578ddd;
		padding: 5px;
	}
	thead th{
		padding: 15px 0px;
	}
	
	tr{
		padding: 160px;
		height: 40px;
		border-bottom: 1px solid rgb(177, 177, 177);
	}
	td{
		padding: 2px;
	}
</style>