<template>

	<div class="col-lg-4 col-md-6 mb-4">
        <div class="p-5 border h-100">
	        <div class="mb-3" style="height: 100px; overflow: hidden; border-radius: 4px;">
                <img 
                :src="project.image"
                class="w-100 h-100"
                style="object-fit: cover;"
                :alt="project.title"
                >
             </div>

            <h3>{{ project.title }}</h3>                      
                <p class="card-text">
                {{ project.description }}</p>

           </div>
      </div>

</template>


<script setup>
	import { defineProps } from 'vue';

	defineProps({
		project: Object
	})
</script>