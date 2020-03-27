<template>
    <q-card>    
        <modal-header>Add task</modal-header>
        <form @submit.prevent="submitForm">
	        <q-card-section class="q-pt-none">

	        	<modal-task-name 
	        		:name.sync="taskToSubmit.name"
	        		ref="ModalTaskName" />

	        	<modal-due-date :dueDate.sync="taskToSubmit.dueDate"/>

		        <modal-due-time 
		        	v-if="taskToSubmit.dueDate"
		        	:dueTime.sync="taskToSubmit.dueTime" />

		        <modal-buttons></modal-buttons>   

	        </q-card-section>        
    	
    	</form>
      </q-card>
    
</template>

<script>
	import { mapActions } from 'vuex'

	export default {
		data(){
			return {
				taskToSubmit :{
					name: '',
					dueDate: '',
					dueTime: '',
					completed: false
				}
			}
		},
		methods: {
			...mapActions('tasks',['addTask']),
			submitForm() {
				this.$refs.ModalTaskName.$refs.name.validate()
				if(!this.$refs.ModalTaskName.$refs.name.hasError){
					this.submitTask()
				}
			},
			submitTask(){
				this.addTask(this.taskToSubmit)
				this.$emit('close')
			}
		},
		components: {
			'modal-header': require('components/tasks/modals/shared/ModalHeader.vue').default,
			'modal-task-name': require('components/tasks/modals/shared/ModalTaskName.vue').default,
			'modal-due-date': require('components/tasks/modals/shared/ModalDueDate.vue').default,
			'modal-due-time': require('components/tasks/modals/shared/ModalDueTime.vue').default,
			'modal-buttons': require('components/tasks/modals/shared/ModalButtons.vue').default
		}

	}
</script>