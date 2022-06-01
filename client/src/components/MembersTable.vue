<template>
  <div class="membersTable">
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
// import Vue from 'vue';
export default {
  name: 'MembersTable',
	// data() {
	// 	return {
	// 	}
	// },
	props:{
		membersArray: []
	},
	}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
	.membersTable{
		position: relative;
		width: fit-content;
		min-width: 60%;
		padding: 25px;
		background-color: #fff;
		box-shadow: 0px 0px 10px rgb(197, 197, 197);
		border-radius: 0.5rem;
	}
	h2{
		margin-bottom: 10px;
	}
	table{
		position: relative;
		width: 100%;
		border-collapse: collapse;
		border-spacing: 0px 15px;
	}
	thead {
		text-align: left;
		border-bottom: solid 2px rgb(66, 141, 211);
	}
	
	tr{
		padding: 16px;
		height: 40px;
		border-bottom: 1px solid rgb(177, 177, 177);
	}
</style>