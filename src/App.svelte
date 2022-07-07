<script lang="ts">
	import Login from './Login.svelte';
	import Logout from './Logout.svelte';
	import Container from './Container.svelte';
	import ContainerSelect from './ContainerSelect.svelte';
	import { DIR_TYPE, FILE_TYPE, type FileInfo } from './container';

	export let name: string;

	let profile: any;
	let selectedResource : FileInfo;
</script>

<nav class="navbar navbar-default">
	<div class="container-fluid">
	  <div class="navbar-header">
		<span class="navbar-brand">{name}</span>
	  </div>
	  <ul class="nav navbar-nav">
		<li>
			<Logout bind:profile={profile}/>
		</li>
	  </ul>
	</div>
</nav>

<Login bind:profile={profile}/>

<Container resource="https://patrickhochstenbach.net/"/>

{#if selectedResource}
 <h3>{selectedResource.url}</h3>
{/if}
<ContainerSelect 
	resource="https://patrickhochstenbach.net/"
	bind:selectedResource
	/>