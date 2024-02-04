<script lang='ts'>
    import type {Post, User} from '../../routes/types';
    import EditTextField from '../system/EditTextField.svelte';
    import ImagePostEditor from '../system/ImagePostEditor.svelte';

    export let post: Post;
    export let user: User;
    export let catName: string;
    export let catTitle: string = '';

    let isEditField:boolean;
    let field:string;
    let isImagePostEdit = false;

    function openEditField(nameField:string){
		isEditField = true;
        field = nameField;
	}

    function imagePostEdit(){
		isImagePostEdit = true;
	}
</script>

<EditTextField bind:isEditField={isEditField} bind:post={post} bind:field={field}/>
{#if isImagePostEdit}
	<ImagePostEditor bind:imagePost={post.image} idPost={post.id} bind:isImagePostEdit={isImagePostEdit} widthImage={380} heightImage={200} imageField='image' />
{/if}

{#if user.isAdmin}
<div class="block">
    <div class="image" on:mousedown="{imagePostEdit}">
        <img src="{post.image}" alt="{post.title}" />
    </div>
    <div class="info">{catTitle}</div>
    <div class="description">
        <div class="title">
            <div class="editfield" on:mousedown={() => openEditField('title')}></div>
            {@html post.title}
        </div>
        <div class="text">
            <div class="editfield" on:mousedown={() => openEditField('text')}></div>
            {@html post.text}
        </div>
        <a href="/{catName}/{post.slug}" class="more">Читать далее</a>
    </div>
</div>
{:else}
<a href="/{catName}/{post.slug}" class="block">
    <div class="image">
        <img src="{post.image}" alt="{post.title}" />
    </div>
    <div class="info">{catTitle}</div>
    <div class="description">
        <div class="title">{@html post.title}</div>
        <div class="text">{@html post.text}</div>
        <div class="more">Читать далее</div>
    </div>
</a>
{/if}

<style lang="scss">
.block{
    display: block;
    text-decoration: none;
    width: calc(100%/3 - 30px);
    margin: 15px;
    transition: .5s;
    border: 1px solid rgba(0,0,0,0.1);
    box-sizing: border-box;
    &:hover{
        transition: .5s;
        box-shadow: 0px 0px 10px 5px rgba(0,0,0,0.1);
        .more{
            color: rgba(0,0,0,1);
            transition: .5s;
        }
    }
}

.image{
    display: flex;
    align-items: center;
    justify-content: center;
    width: 100%;
    min-height: 100px;
    max-height: 200px;
    overflow: hidden;
    background-color: rgba(0,0,0,0.1);
    img{
        width: 100%;
        max-width: 100%;
        height: auto;
        vertical-align: bottom;
    }
}

.info{
    display: flex;
    justify-content: center;
    width: 100%;
    box-sizing: border-box;
    padding: 7px 14px;
    background-color: #000;
    color: #fff;
    font-size: 14px;
}

.description{
    width: 100%;
    box-sizing: border-box;
    padding: 20px 15px;
}

.title{
    position: relative;
    width: 100%;
    box-sizing: border-box;
    color: rgba(0,0,0,1);
    font-size: 16px;
    line-height: 1.5em;
    font-weight: 600;
}

.text{
    position: relative;
    width: 100%;
    box-sizing: border-box;
    color: rgba(0,0,0,0.75);
    font-size: 14px;
    line-height: 1.5em;
    margin-top: 15px;
}

.more{
    display: inline-block;
    width: 100%;
    box-sizing: border-box;
    color: rgba(0,0,0,0.7);
    font-weight: 600;
    margin-top: 15px;
    transition: .5s;
    text-decoration: none;
}

.editfield{
    position: absolute;
    z-index: 1;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    cursor: pointer;
}

@media only screen and (max-width: 1023px){
    .block{
        width: calc(100%/2 - 30px);
    }	
}

@media only screen and (max-width: 639px){
    .block{
        width: calc(100% - 30px);
    }	
}
</style>