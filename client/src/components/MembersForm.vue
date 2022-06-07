<template>
  <div class="membersForm">
		<h2>Create a member</h2>
		<form >
			<div class="inputs-wrapper">
				<input
				type="text"
				v-model.trim.lazy="$v.dataForm.firstName.$model"
				@blur="handleURLInput('firstName')"
				:class="{'invalid' : $v.dataForm.firstName.$error}"
				placeholder="First Name"
				/>
				<div class="errorMessages" v-if="!$v.dataForm.firstName.minLength">At least 2 characters</div>
				<div class="errorMessages" v-if="!$v.dataForm.firstName.required && $v.dataForm.firstName.$dirty">Field Required</div>
				<input
				type="text"
				v-model.trim.lazy="$v.dataForm.lastName.$model"
				@blur="handleURLInput('lastName')"
				:class="{'invalid' : $v.dataForm.lastName.$error}"
				placeholder="Last Name"
				/>
				<div class="errorMessages" v-if="!$v.dataForm.lastName.minLength">At least 2 characters</div>
				<div class="errorMessages" v-if="!$v.dataForm.lastName.required && $v.dataForm.lastName.$dirty">Field Required</div>
				<input
				type="text"
				v-model.trim.lazy="$v.dataForm.address.$model"
				@blur="handleURLInput('address')"
				:class="{'invalid' : $v.dataForm.address.$error}"
				placeholder="Adress"
				/>
				<div class="errorMessages" v-if="!$v.dataForm.address.minLength">At least 2 characters</div>
				<div class="errorMessages" v-if="!$v.dataForm.address.required && $v.dataForm.address.$dirty">Field Required</div>
				<input
				v-model.trim.lazy="$v.dataForm.ssn.$model"
				@blur="handleURLInput('ssn')"
				:class="{'invalid' : $v.dataForm.ssn.$error}"
				placeholder="SSN"
				/>
				<div class="errorMessages" v-if="!$v.dataForm.ssn.minLength || !$v.dataForm.ssn.format">SSN format: * NNN-NN-NNNN<br>*N: number</div>
				<div class="errorMessages" v-if="!$v.dataForm.ssn.required && $v.dataForm.ssn.$dirty">Field Required</div>
			</div>
			
			<div class="buttons-form-wrapper">
				<div @click="resetForm" class="buttons reset">Reset</div>
				<div @click="sendMember" :class="['buttons', 'active', {'inactive' : !isValidData}]">Send</div>
			</div>
		</form>
  </div>
</template>
<script>
import { required, minLength, maxLength, helpers} from 'vuelidate/lib/validators';
export default {
  name: 'MembersForm',
	props: {
		members:[],
	},
	data() {
		return {
			dataForm: {
				firstName: '',
				lastName: '',
				address: '',
				ssn: '',
			}
		}
	},
	validations() {
		return {
			dataForm: {
				firstName: {
					required,
					minLength: minLength(2)
				},
				lastName: {
					required,
					minLength: minLength(2)
				},
				address: {
					required,
					minLength: minLength(2)
				},
				ssn: {
					required,
					minLength: minLength(11),
					maxLength: maxLength(11),
					validator(value){
						return !this.existingMembersSSN.includes(value);
					},
					format(value) {
						const alpha = helpers.regex('alpha', /^\d{3}-\d{2}-\d{4}$/);
						return alpha(value);
					}
				}
			}
		}
	},
	
	computed: {
		isValidData() {
			return !this.$v.dataForm.$invalid? true : false;
		},
		existingMembersSSN() {
			let array = [];
			if(this.members.length) {
				this.members.forEach((member) => {
					array.push(member.ssn);
				});
			}
			return array;
		}
		
	},
	watch:{
		members: {
			deep: true,
			immediate: false,
			handler(){
				this.resetForm();
			}
		}
	},
	methods:{
		handleURLInput(field) {
			// Tell vuelidate that the field has been changed so it can set `field.$dirty` to true and we can display the error
			this.$v.dataForm[field].$touch();
		},
		sendMember() {
			if(!this.$v.$anyError){	
				this.$emit('sendMember', this.dataForm);
			}
		},
		resetForm() {
			for (const property in this.dataForm){
				this.dataForm[property] = '';
			}
			this.$v.$reset();
		}
	}
}
</script>

<style scoped>
	.membersForm{
		position: relative;
		width: 30%;
		display: flex;
		flex-direction: column;
		background-color: #fff;
		padding: 20px 25px;
		border-radius: 0.5rem;
		box-shadow: 0px 0px 10px rgb(197, 197, 197);

	}
	h2{
		margin-bottom: 20px;
		font-size: 1.2em;
	}
	form {
		position: relative;
		display: flex;
		flex-direction: column;
		justify-content: space-between;
		height: fit-content;
		width: 100%;
	}
	.inputs-wrapper{
		position: relative;
		height: fit-content;
	}
	input {
		box-sizing: border-box;
		width: 100%;
		margin: 5px 0;
		padding: 10px;
		border: solid 1.5px #cfcfcf;
		outline: none;
		color: #272727;
		font-weight: 500;
		border-radius: 5px;
		transition: all 0.3s;
		}
		input::placeholder {
			color: #969696;
		}
	.errorMessages{
		color: #d64c4c;
		font-size: 0.9em;
	}
	.buttons-form-wrapper{
		display: flex;
		justify-content: space-between;
		margin-top: 25px;
		bottom: 0;
		position: relative;
	}
	.buttons{
		cursor: pointer;
		padding: 5px 10px;
		border-radius: 5px;
		transition: all 0.2s;
		border: 2px solid #578ddd;
	}
	.reset:hover{
		background: #578ddd;
		color: #fff;
	}
	.invalid{
		border-color: #d64c4c;
	}
	.active{
		background: #578ddd;
		color: #fff;
		width: 120px;
		max-width: 50%;
		text-align: center;
	}
	.active:hover{
		background: #57dda5;
		border-color: #57dda5;
	}
	.inactive{
		pointer-events: none;
		opacity: 0.5;
	}
</style>