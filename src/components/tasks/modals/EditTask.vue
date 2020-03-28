<template>
    <q-card>    
        <modal-header>Edit task</modal-header>
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
		props: ['task', 'id'],
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
			...mapActions('tasks',['updateTask']),
			submitForm() {
				this.$refs.ModalTaskName.$refs.name.validate()
				if(!this.$refs.ModalTaskName.$refs.name.hasError){
					this.submitTask()
				}
			},
			submitTask(){
				this.updateTask({
					id: this.id,
					updates: this.taskToSubmit
				})
				this.$emit('close')
			}
		},
		components: {
			'modal-header': require('components/tasks/modals/shared/ModalHeader.vue').default,
			'modal-task-name': require('components/tasks/modals/shared/ModalTaskName.vue').default,
			'modal-due-date': require('components/tasks/modals/shared/ModalDueDate.vue').default,
			'modal-due-time': require('components/tasks/modals/shared/ModalDueTime.vue').default,
			'modal-buttons': require('components/tasks/modals/shared/ModalButtons.vue').default
		},
		mounted() {
			this.taskToSubmit = Object.assign({}, this.task)
		}

	}
</script>