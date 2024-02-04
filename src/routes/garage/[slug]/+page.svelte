<script lang='ts'>
	import type {OG, User, EditUrl, Post} from '../../types';
	import { goto } from '$app/navigation';

	import Popup from '../../../components/system/Popup.svelte';
	import AdminButtons from '../../../components/system/AdminButtons.svelte';
	import BtnAdminEdit from '../../../components/system/BtnAdminEdit.svelte';
	import OpenGraphEditor from '../../../components/system/OpenGraphEditor.svelte';
	import UploaderFiles from '../../../components/system/UploaderFiles.svelte';
	import UploaderImages from '../../../components/system/UploaderImages.svelte';
	import Button from '../../../components/system/Button.svelte';
	import Switch from '../../../components/system/Switch.svelte';
	import PostNodesEditor from '../../../components/system/PostNodesEditor.svelte';
	import PostNodesShow from '../../../components/system/PostNodesShow.svelte';

	import vs2015 from "svelte-highlight/styles/vs2015";

	export let data:{
		post: Post;
		og_data: OG[];
		user: User;
	};

	let post = data.post;
	let og_data = data.og_data[0];
	let user = data.user;

	let edit_url:EditUrl = {
		popup: false,
		url: '',
	};

	let isOpenGraphEdit = false;
	let isUploadFile:boolean;
	let isUploadImages:boolean;


	async function savePostFn(){

		for (let node of post.post){
            delete node.isEdit;
        }

		const response = await fetch('/api/updatepost', {
			method: 'POST',
			headers: {
				'content-type': 'application/json'
			},
			body: JSON.stringify({ post: post.post, id: post.id }),
		});

	}

	function openGraphEdit(){
		isOpenGraphEdit = true;
	}

	function isuploadCss(){
		isUploadFile = true;
	}

	function isuploadImages(){
		isUploadImages = true;
	}

	function urlEdit(url:string){
		edit_url.popup = true;
		edit_url.url = url;
	}

	async function saveUrl(url:string){
		const response = await fetch('/api/getcount', {
			method: 'POST',
			headers: {
				'content-type': 'application/json'
			},
			body: JSON.stringify({ slug: url }),
		});

		let count:number = await response.json();

		if(count > 0){
			console.log('Запись с таким url: '+url+' уже существует!');
		}else{
			const response = await fetch('/api/updateslug', {
				method: 'POST',
				headers: {
					'content-type': 'application/json'
				},
				body: JSON.stringify({ slug: url, id: post.id }),
			});
			//let total = await response.json();
			goto('/blog/'+url);
			edit_url.popup = false;
			post.slug = url;
		}
	}

	async function changeStatus(){

		post.publication = !post.publication;

		const response = await fetch('/api/update', {
			method: 'POST',
			headers: {
				'content-type': 'application/json'
			},
			body: JSON.stringify({ id: post.id, field: 'publication', value: post.publication  }),
		});

		let res = await response.json();

		//console.log(res);
	}

	const monthNames = ["Январь", "Февраль", "Март", "Апрель", "Май", "Июнь", "Июль", "Август", "Сентябрь", "Октябрь", "Ноябрь", "Декабрь"];

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
	{#if post.includecss}
		<link rel="stylesheet" href="{post.includecss}">
	{/if}
	{@html vs2015}
</svelte:head>

<Popup bind:isOpen={edit_url.popup}>
	<slot slot="title">
		Редактировать url поста
	</slot>
	<slot slot="content">
		<input class="editUrl" type="text" bind:value={edit_url.url} />
	</slot>
	<slot slot="bottom">
		<Button title="Сохранить" on:click="{() => saveUrl(edit_url.url)}" />
	</slot>
</Popup>

{#if isOpenGraphEdit}
	<OpenGraphEditor bind:dataOpenGraph={og_data}  bind:isOpenGraphEdit={isOpenGraphEdit} />
{/if}

{#if isUploadFile}
	<UploaderFiles bind:isUploadFile={isUploadFile} bind:post={post} typeUploadFile='css' fieldPost='includecss' />
{/if}

{#if isUploadImages}
	<UploaderImages bind:isUploadImages={isUploadImages} bind:includeFiles={post.includefiles} idPost={post.id}  />
{/if}

{#if user.isAdmin}
<AdminButtons>
	<BtnAdminEdit title="OpenGraph" bg="opengraph" on:click="{openGraphEdit}" />
	<BtnAdminEdit title="URL" bg="url" on:click="{() => urlEdit(post.slug)}" />
	<BtnAdminEdit title="Icon" bg="css" on:click="{isuploadCss}" />
	<BtnAdminEdit title="Gallery" bg="gallery" on:click="{isuploadImages}" />
</AdminButtons>
{/if}

<div class="wrap wrap_post">
	<div class="work">
		<div class="container">
			<article class="post">
				{#if user.isAdmin}<div class="publication"><Switch status={post.publication} on:click={changeStatus} /></div>{/if}
	
				<h1 class="title">
					{@html post.title}
				</h1>
				{#if user.isAdmin}
				<div class="btns">
					<button class="btn save" on:click={savePostFn}>Сохранить</button>
				</div>
				{/if}
				{#if user.isAdmin}
					<PostNodesEditor bind:nodes={post.post} />
				{:else}
					<PostNodesShow bind:nodes={post.post} />
				{/if}
			</article>
			<div class="sidebar">
				<div class="widget">
					<h2 class="widget_title">Смотрите также:</h2>
					<div class="widget_content">
						<div class="widget_line">
							<div class="w_image">
								<img src="/tech/no-image.jpg" alt="" width="100" height="100" />
							</div>
							<div class="w_info">
								<div class="w_title">Многоразовые чехлы для колес против шуршащих пакетов: выбираем удобство</div>
								<div class="w_link">Читать далее</div>
							</div>
						</div>
						<div class="widget_line">
							<div class="w_image">
								<img src="/tech/no-image.jpg" alt="" width="100" height="100" />
							</div>
							<div class="w_info">
								<div class="w_title">Многоразовые чехлы для колес против шуршащих пакетов: выбираем удобство</div>
								<div class="w_link">Читать далее</div>
							</div>
						</div>
						<div class="widget_line">
							<div class="w_image">
								<img src="/tech/no-image.jpg" alt="" width="100" height="100" />
							</div>
							<div class="w_info">
								<div class="w_title">Многоразовые чехлы для колес против шуршащих пакетов: выбираем удобство</div>
								<div class="w_link">Читать далее</div>
							</div>
						</div>
					</div>
				</div>
			</div>
		</div>
	</div>
</div>

<style lang="scss">
.wrap_post{
	padding: 40px 0 40px 0;
}

.container{
	display: flex;
	flex-wrap: wrap;
	width: 100%;
}

.btns{
	width: 100%;
	margin: 20px 0;
	display: flex;
	justify-content: flex-end;
}

.btn{
	padding: 7px 14px;
	background-color: #000;
	border: none;
	outline: none;
	color: #fff;
	margin-left: 10px;
	cursor: pointer;
	font-size: 12px;
	&:hover{
		background-color: rgba(0,0,0,0.75);
	}
}

.post{
	position: relative;
	width: calc(100% - 360px);
	box-sizing: border-box;
	padding-right: 40px;
}

.title{
	position: relative;
    width: 100%;
    color: var(--text-primary);
    text-align: left;
    font-size: 28px;
	font-weight: 500;
	margin-bottom: 25px;
}

.editUrl{
	width: 100%;
	box-sizing: border-box;
	outline: none;
	border: 1px solid #000;
	padding: 7px 10px;
}

.sidebar{
	width: 360px;
}

.widget{
	width: 100%;
}

.widget_title{
	width: 100%;
	font-weight: 500;
	font-size: 16px;
	text-transform: uppercase;
}

.widget_content{
	width: 100%;
	margin-top: 10px;
}

.widget_line{
	display: flex;
	width: 100%;
	padding: 10px 0;
	margin-bottom: 10px;
	border-bottom: 1px solid rgba(0,0,0,0.1);
}

.w_image{
	width: 100px;
	min-width: 100px;
	height: 100px;
	overflow: hidden;
	background-color: rgba(0,0,0,0.1);
	img{
		vertical-align: bottom;
	}
}

.w_info{
	margin-left: 10px;
}

.w_title{
	width: 100%;
	font-weight: 500;
	font-size: 16px;
	line-height: 1.2em;
}

.w_link{
	width: 100%;
	font-size: 16px;
	line-height: 1em;
	margin-top: 10px;
}

@media only screen and (max-width: 1024px){
	.post{
		width: 100%;
		padding-right: 0;
	}

	.sidebar{
		width: 100%;
		margin-top: 40px;
	}
}

@media only screen and (max-width: 639px){
	.title{
		font-size: 22px;
		line-height: 1.5em;
	}
}
</style>