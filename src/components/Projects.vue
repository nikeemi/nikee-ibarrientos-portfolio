<template>
	<!--My Project Section Start-->
<section id="projects" class="container my-5">
	
    <div class="row">
		<div class="text-center pb-3">
		<h2>My Projects</h2>

	        <div v-for="(group, index) in chunkedProjects" :key="index" class="card-deck my-5 justify-content-center">
            <ProjectCard v-for="project in group" :key="project.id" :project="project" />
            </div>
         </div>
    </div>
		
</section>
<!--My Project Section End-->
</template>

<script setup>
	import { computed } from 'vue';
	import ProjectCard from './ProjectCard.vue';
	import projects from '../data/projects.json';

	const chunkSize = 3;

	const chunkedProjects = computed(() => {
		const chunks = [];

		for(let i = 0; i < projects.length; i += chunkSize) {
			chunks.push(projects.slice(i, i + chunkSize));
		}
		return chunks;
	})
</script>