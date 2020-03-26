<template>
	
      <q-card>
        <q-card-section class="row">
          <div class="text-h6">Add Task</div>
          <q-space />
          <q-btn 
          	v-close-popup
          	flat 
          	round 
          	dense 
          	icon="close" />
        </q-card-section>

        <form @submit.prevent="submitForm">
	        <q-card-section class="q-pt-none">

	        	<div class="row q-mb-sm">
	        	<q-input 
		          	outlined 
		          	v-model="taskToSubmit.name" 
		          	:rules="[val => !!val || 'Field is required']"
		          	autofocus
		          	ref="name"
		          	label="Task name" 
		          	class="col">
		          	<template v-slot:append>
          				<q-icon
          					v-if="taskToSubmit.name" 
          					name="close" 
          					@click="taskToSubmit.name = ''" 
          					class="cursor-pointer" />
        			</template>
		        </q-input>
		        </div>
		        <div class="row q-mb-sm">
			  	<q-input 
			  		outlined
			  		label="Due date"
			     	v-model="taskToSubmit.dueDate" 
			    	 mask="date">
			  		<template v-slot:append>
			    	<q-icon name="event" class="cursor-pointer">
			    		<q-popup-proxy ref="qDateProxy" transition-show="scale" transition-hide="scale">
			         	<q-date v-model="taskToSubmit.dueDate" @input="() => $refs.qDateProxy.hide()" />
			         	</q-popup-proxy>
			     	</q-icon>
			     	</template>
			   	</q-input>
			   </div>
			   
			   <div 
			   	v-if="taskToSubmit.dueDate"
			   	class="row q-mb-sm">
					<q-input 
						outlined 
						label="Due time"
						v-model="taskToSubmit.dueTime">
				        <template v-slot:append>
				          <q-icon name="access_time" class="cursor-pointer">
				            <q-popup-proxy transition-show="scale" transition-hide="scale">
				              <q-time v-model="taskToSubmit.dueTime" />
				           	</q-popup-proxy>
				    		</q-icon>
				    	</template>
				    </q-input>
				</div>
	        </q-card-section>

	        <q-card-actions align="right">
	          <q-btn 
	          	label="Save" 
	          	color="primary" 
	          	type="submit" />
	        </q-card-actions>
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
				this.$refs.name.validate()
				if(!this.$refs.name.hasError){
					this.submitTask()
				}
			},
			submitTask(){
				this.addTask(this.taskToSubmit)
				this.$emit('close')
			}
		}

	}
</script>