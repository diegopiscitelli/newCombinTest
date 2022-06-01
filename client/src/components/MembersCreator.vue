<template>
  <div class="membersCreator">
		<h1>Members</h1>
		<div class="membersCreator--subwrapper">
			<MembersForm @sendMember="postMember" :members="members" :postMemberSucces="postMemberSended"/>
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
			postMemberSended: false
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
		background-color: rgb(225, 226, 231);
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		height: 100%;
		width: 100%;
		text-align: left;
		padding: 10px;
	}
	h1{
		align-self: flex-start;
		padding: 5px 50px;
	}
	.membersCreator--subwrapper{
		position: relative;
		display: flex;
		/* flex-wrap: wrap; */
		justify-self: center;
		/* max-width: 90%; */
		justify-content: space-around;
		width: 95%;
		min-height: 600px;
		padding: 50px 25px;
		background-color: rgb(240, 240, 240);
		border-radius: 5px;
	}
</style>