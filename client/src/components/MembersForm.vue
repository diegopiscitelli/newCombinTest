<template>
  <div class="membersForm">
		<form >
			<input v-model="dataForm.firstName" @blur="handleURLInput('firstName')" :class="{'invalid' : $v.dataForm.firstName.$error}" placeholder="First Name"/>
			<input v-model="dataForm.lastName" @blur="handleURLInput('lastName')" :class="{'invalid' : $v.dataForm.lastName.$error}" placeholder="Last Name"/>
			<input v-model="dataForm.address" @blur="handleURLInput('address')" :class="{'invalid' : $v.dataForm.address.$error}" placeholder="Adress"/>
			<input v-model="dataForm.ssn" @blur="handleURLInput('ssn')" :class="{'invalid' : $v.dataForm.ssn.$error}" placeholder="SSN"/>
			<div v-if="!$v.dataForm.$anyError">Valid</div>
			<div class="buttons-form-wrapper">
				<div @click="resetForm" class="buttons reset">Reset</div>
				<div @click="sendMember()" :class="['buttons', 'active', {'inactive' : !isValidData}]">Send</div>
			</div>
		</form>
  </div>
</template>

<script>
import { required, minLength} from 'vuelidate/lib/validators';
export default {
  name: 'MembersForm',
	props: {
		members:[]
	},
	data() {
		return {
			dataForm: {
				firstName: '',
				lastName: '',
				address: '',
				ssn: '',
			},
			// members:[]
			// validData: {
			// 	firstName:'',
			// 	lastName:'',
			// 	address:'',
			// 	ssn:'',
			// },
			// isValidData: this.$validations.dataForm.$error? true : false
		}
	},
	validations() {
		return {
			dataForm: {
				firstName: {
					required,
					minLength: minLength(1)
				},
				lastName: {
					required,
					minLength: minLength(1)
				},
				address: {
					required,
					minLength: minLength(1)
				},
				ssn: {
					required,
					minLength: minLength(11),
					validator(value){
						console.log(this.existingMembersSSN.includes(value));
						return !this.existingMembersSSN.includes(value);
					}
				}
			}
		}
	},
	computed: {
		isValidData() {
			console.log('polla');
			return !this.$v.dataForm.$invalid? true : false;
		},
		existingMembersSSN() {
			let array = [];
			if(this.members.length) {
				this.members.forEach((member) => {
					array.push(member.ssn);
					console.log(array);
				});
			}
			return array;
		}
		
	},
	methods:{
		// validData() {
		// 	// let data = this.dataForm;
		// },
		
		handleURLInput(field) {
			// Tell vuelidate that the url has been changed so it can set `url.$dirty` to true and we can display the error
			this.$v.dataForm[field].$touch();
		},
		sendMember() {
			this.$emit('sendMember', this.dataForm);
		},
		resetForm() {
			for (const property in this.dataForm){
				this.dataForm[property] = '';
			}
		}
	}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
	.membersForm{
		position: relative;
		width: 35%;
		display: flex;
		flex-direction: column;
		background-color: #fff;
		padding: 25px;
		border-radius: 0.5rem;
		box-shadow: 0px 0px 10px rgb(197, 197, 197);

	}
	form {
		display: flex;
		flex-direction: column;
	}
	input {
		box-sizing: border-box;
		width: 100%;
		margin: 2px 0;
		padding: 10px;
		border: solid 1px rgb(207, 207, 207);
		outline: none;
		color: rgb(39, 39, 39);
		font-weight: 500;
		border-radius: 5px;
		transition: all 0.2s;
		}
		input::placeholder {
			color: rgb(150, 150, 150);
		}
	.buttons-form-wrapper{
		display: flex;
		justify-content: space-between;
		margin-top: 25px;
	}
	.buttons{
		cursor: pointer;
		box-shadow: 0px 0px 4px rgb(197, 197, 197);
		padding: 5px 10px;
		border-radius: 5px;
		transition: all 0.2s;

	}
	.invalid{
		border-color: red;
	}
	.active{
		background: rgb(66, 141, 211);
		color: #fff;
	}
	.inactive{
		pointer-events: none;
		opacity: 0.5;
	}
</style>