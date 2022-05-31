<template>
  <div class="membersCreator">
		<div class="membersCreator--subwrapper">
			<MembersForm @sendMember="postMember" :members="members"/>
			<MembersTable :membersArray="members"/>
		</div>
  </div>
</template>

<script>
import MembersForm from './MembersForm.vue';
import MembersTable from './MembersTable.vue';

export default {
  name: 'MembersCreator',
	components: {
		MembersForm,
		MembersTable
	},
	data() {
		return {
			token: {},
			members: [],
		}
	},
	
	mounted() {
		this.getMembers();
	},

	methods: {
		async getToken() {
			try {
				let auth = {
					username:'sarah',
					password:'connor'
				};
				let status;
				const res = await fetch('http://localhost:8081/auth', {
					method: 'POST',
					body: JSON.stringify(auth),
					headers: {
						'Content-Type': 'application/json;charset=utf-8',
					}
				})
				.then((response) => {
					status = response.status;
					return response.json();
				})
				.then((responseData) => {
					return responseData;
				});
				if (status === 200) {
					this.token = res;
				}

			}catch (error) {
				console.log(error);
			}
		},
		async postMember(member) {
			try {
				await this.getToken();
				let auth = this.token.token;
				let status;
				const res = await fetch('http://localhost:8081/api/members', {
					method: 'POST',
					body: JSON.stringify(member),
					headers: {
						'Content-Type': 'application/json;charset=utf-8',
						'Authorization': `Bearer ${auth}`
					}
				})
				.then((response) => {
					status = response.status;
					return response.json();
				})
				.then((responseData) => {
					return responseData;
				});
				if (status === 200) {
					this.members.push(res);
				}

			}catch (error) {
				console.log(error);
			}
		},
		async getMembers() {
			await this.getToken();
			let auth = this.token.token;
			let status;
			const res = fetch('http://localhost:8081/api/members', {
				method: 'GET',
				headers: {
					'Authorization': `Bearer ${auth}`
				},
				cache: 'default'
			})
			.then( (res) => {
				status = res.status;
				return res.json()
			})
			.then( (json) => {
				return json;
				})
			let data = await res;
			if(status === 200 ){
				this.members = data;
			}
		}
	}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
	.membersCreator{
		position: relative;
		background-color: rgb(123, 139, 139);
		display: flex;
		justify-content: center;
		padding: 50px;
	}
	.membersCreator--subwrapper{
		position: relative;
		display: flex;
		justify-self: center;
		justify-content: space-between;
		max-width: 90%;
		padding: 25px;
		background-color: rgb(240, 240, 240);
		border-radius: 5px;
	}
</style>