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
			time: null,
			activityInterval: null,
		}
	},
	
	mounted() {
		this.getMembers();
		this.time = new Date().getTime();
		// listeners to detect user activity
		window.addEventListener('click', this.setActivityTime); 
		window.addEventListener('scroll', this.setActivityTime); 
		window.addEventListener('mousemove', this.setActivityTime);
		window.addEventListener('keydown', this.setActivityTime); 
	},
	
	beforeDestroy() {
		window.removeEventListener('click', this.setActivityTime); 
		window.removeEventListener('scroll', this.setActivityTime); 
		window.removeEventListener('mousemove', this.setActivityTime);
		window.removeEventListener('keydown', this.setActivityTime); 
	},

	watch:{
		time(newValue, oldValue) {
			if(!newValue || !oldValue || this.isLoadingData) return;
			if(newValue - oldValue >= 120000) {
				this.getMembers();
			}
		}
	},
	methods: {
		setActivityTime() {
			if (this.activityInterval){
				this.time = new Date().getTime();
				clearInterval(this.activityInterval);
			}
			this.activityInterval = setInterval(this.setActivityTime, 120000);
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
				console.log(json);
				return json;
				})
			let data = await res;
			if(status === 200 ){
				this.members = data;
				this.isLoadingData = false;
				this.setActivityTime();
			}
		}
	}
}
</script>

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
		margin-left: 5px;
		align-self: center;
		width: 85%;
		height: 35px;
		padding: 5px 10px;
		border-left: 3px solid #578ddd;
		margin-bottom: 15px;
		font-size: 1.2em;
	}
	.membersCreator--subwrapper{
		position: relative;
		display: flex;
		justify-self: center;
		justify-content: space-around;
		height: 85%;
		width: 85%;
		padding: 50px 25px;
		background-color: #fafafa;
		border-radius: 10px;
	}
</style>