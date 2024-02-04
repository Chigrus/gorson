<script lang='ts'>
	import type {OG, User, Posts} from '../types'

	import AdminButtons from '../../components/system/AdminButtons.svelte';
	import BtnAdminEdit from '../../components/system/BtnAdminEdit.svelte';
	import OpenGraphEditor from '../../components/system/OpenGraphEditor.svelte';
	import Card from "../../components/cats/Card.svelte";

	export let data:{
		posts: Posts;
		og_data: OG[];
		user: User;
	};

	let posts = data.posts;
	let og_data = data.og_data[0];
	let user = data.user;

	let isOpenGraphEdit = false;

	function openGraphEdit(){
		isOpenGraphEdit = true;
	}
</script>

<svelte:head>
	<title>{og_data.title}</title>
	<meta name="description" content="{og_data.description}" />
	<meta property="og:type" content="{og_data.og_type}" />
	<meta property="og:title" content="{og_data.og_title}" />
	<meta property="og:description" content="{og_data.og_description}" />
	<meta property="og:url" content="{og_data.og_url}">
	<meta property="og:image" content="{og_data.og_image}">
	<meta property="og:image:type" content="{og_data.og_image_type}" />
	<meta property="og:image:width" content="{og_data.og_image_width}">
	<meta property="og:image:height" content="{og_data.og_image_height}">
	<meta name="twitter:title" content="{og_data.title}">
	<meta name="twitter:description" content="{og_data.description}">
</svelte:head>

{#if user.isAdmin}
<AdminButtons>
	<BtnAdminEdit title="OpenGraph" bg="opengraph" on:click="{openGraphEdit}" />
</AdminButtons>
{/if}

{#if isOpenGraphEdit}
	<OpenGraphEditor bind:dataOpenGraph={og_data}  bind:isOpenGraphEdit={isOpenGraphEdit} />
{/if}


<div class="wrap wrap_gadgets">
	<div class="work">
		<div class="gadgets">
			{#each posts as post}
				<Card post={post} {user} catName="gadgets" catTitle="Гаджеты" />
			{/each}
		</div>
	</div>
</div>

<style lang="scss">
.gadgets{
	width: calc(100% + 30px);
	margin-left: -15px;
	display: flex;
	flex-wrap: wrap;
}
</style>