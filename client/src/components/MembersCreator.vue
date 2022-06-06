<template>
  <div ref="creator" class="membersCreator">
		<h1>Members Dashboard</h1>
		<div class="membersCreator--subwrapper">
			<MembersForm @sendMember="postMember" :members="members"/>
			<MembersTable :membersArray="members" :loading="isLoadingData"/>
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
			isLoadingData: false,
			activity: false,
			activityInterval: null,
			time: null,
		}
	},
	
	mounted() {
		this.getMembers();
		this.time = new Date().getTime();
		// this.activityInterval = setInterval(this.getMembers, 5000);
		this.$refs.creator.addEventListener('mousemove', this.setActivityTime);
		// setTimeout(() => {
		// 	this.refresh();
		// }, 10000);
	},
	watch:{
		// activity(value) {
		// 	if (!value) {
		// 		clearInterval(this.activityInterval);
		// 		this.activityInterval = null;
		// 		this.getMembers();
		// 	}
		// }
	},
	methods: {
		// resetActivity() {
		// 	console.log('caca');
		// 	this.activityInterval = setInterval(this.getMembers, 10000);
		// 	this.activity = true;
		// },
		setActivityTime() {
			console.log('mousmove');
			this.time = new Date().getTime();
			this.refresh();
		},
		refresh() {
			console.log(new Date().getTime() - this.time >= 15000);
			if (new Date().getTime() - this.time >= 15000) {
				console.log('refresh');
				console.log('inactivity');
				this.getMembers();
			} else {
				console.log('refresh again');
				setTimeout(() => {
					this.refresh();
				}, 1000);
			}
		},
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
					this.setActivityTime();
				}

			}catch (error) {
				console.log(error);
			}
		},
		async getMembers() {
			await this.getToken();
			this.activity = true;
			let auth = this.token.token;
			this.isLoadingData = true;
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
				this.isLoadingData = false;
			}
		}
	}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
	.membersCreator{
		position: relative;
		background-color: #fff;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		width: 100%;
		height: 100%;
		text-align: left;
		padding: 10px 50px;
	}
	h1{
		align-self: flex-start;
		padding: 5px 10px;
		border-left: 3px solid #578ddd;
		margin-bottom: 15px;
		font-size: 1.2em;
	}
	.membersCreator--subwrapper{
		position: relative;
		display: flex;
		justify-self: center;
		/* max-width: 90%; */
		justify-content: space-around;
		height: 100%;
		width: 80%;
		padding: 50px 25px;
		background-color: #f7f7f7;
		border-radius: 10px;
	}
</style>