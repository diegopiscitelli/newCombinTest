<template>
  <div class="membersTable">
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
	}
</style>