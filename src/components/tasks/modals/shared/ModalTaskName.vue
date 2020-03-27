<template>
	<div class="row q-mb-sm">
	    <q-input 
		  	outlined 
		   	:value="name"
		   	@input="$emit('update:name', $event)"
		    :rules="[val => !!val || 'Field is required']"
		    autofocus
		    ref="name"
		    label="Task name" 
		    class="col">
		    <template v-slot:append>
          		<q-icon
          			v-if="name" 
          				name="close" 
          				@click="$emit('update:name', '')" 
          				class="cursor-pointer" />
        	</template>
		</q-input>
	</div>
</template>
<script>
	export default {
		props: ['name']
	}
</script>