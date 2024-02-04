<script lang='ts'>
    export let position:boolean;
    export let url:string;
    export let alt:string;
    export let width:number;
    export let height:number;
    export let text:string;

    import { cropImage } from '../../domain';
    import { uploadImage } from '../../requests';

    import type { SvelteComponent, ComponentType } from 'svelte';

    import Popup from './Popup.svelte';
    import Button from './Button.svelte';
    import Editor from 'cl-editor';
    import Cropper from "svelte-easy-crop";
    import ButtonUploadFile from './ButtonUploadFile.svelte';
    import SwitchSimple from './SwitchSimple.svelte';
    
    // @ts-ignore
    const cedit: ComponentType<SvelteComponent> = Editor;

    let isEdit = false;
    let typeData:string;
    const actions = ['b', 'i', 'undo', 'redo', ];
    let cropData:Object;
    $: cropSize = {width: width, height: height};
    let isUploadImage:boolean;

    function openEdit(type:string){
        typeData = type;
		isEdit = true;
	}

    async function saveChange(){
        if(typeData === 'img'){
            if (isUploadImage){
                const blob = await cropImage(url, cropData, cropSize, 'image/jpeg', 1);
                let res = await uploadImage('image/jpeg', blob);
                url = res.url;
            }
        }
		isEdit = false;
	}

    function editHtml(e:CustomEvent){
        text = e.detail;
    }

    function changePosition(){
        position = !position;
    }
</script>

<Popup bind:isOpen="{isEdit}">
	<div slot="title">Редактировать</div>
    <div slot="content">
        {#if typeData === 'text'}
            <svelte:component this={cedit} actions={actions} html={text} on:change="{(e) => editHtml(e)}" />
        {/if}
        {#if typeData === 'img'}
            <div class="cropper" style="height: {height+50}px">
                <Cropper 
                    image={url}
                    crop={{ x: 0, y: 0 }}
                    zoom={1}
                    minZoom={0.8}
                    maxZoom={2}
                    cropSize = {{width: width, height: height}}
                    zoomSpeed={0.1}
                    restrictPosition={false}
                    on:cropcomplete={(event) => cropData = event.detail.pixels}
                />
            </div>
            <div class="imgInfo">
                <ButtonUploadFile bind:imgFile={url} bind:imgUpload={isUploadImage} />
                <input class="inp_w" type="number" bind:value={width} />
                <input class="inp_h" type="number" bind:value={height} />
                <SwitchSimple bind:status={position} on:click={changePosition} />
                <input class="inp_alt" type="text" bind:value={alt} placeholder="Alt" />
            </div>
        {/if}
    </div>
    <div slot="bottom">
		<div class="lineSave">
			<Button title="Сохранить" on:click="{saveChange}" />
		</div>
	</div>
</Popup>

<div class="container">
    <div class="inner">
        <span on:mousedown={() => openEdit('img')}>
            <img 
                class="image_text {position ? 'left' : 'right'}" 
                src="{url}" 
                alt={alt} 
                width="{width}" 
                height="{height}"  
            />
        </span>
        <span on:mousedown={() => openEdit('text')}>
            {@html text}
        </span>
    </div>
</div>


<style lang="scss">
.container{
    position: relative;
    width: 100%;
    box-shadow: 0 2px 3px rgba(10, 10, 10, 0.1), 0 0 0 1px rgba(10, 10, 10, 0.1);
    background-color: #ecf0f1;
    padding-top: 40px;box-sizing: border-box;
}

.inner{
    width: 100%;
    background-color: #fff;
    border-top: 1px solid rgba(10, 10, 10, 0.1);
    box-sizing: border-box;
    padding: 10px;
}

.image_text{
    vertical-align: bottom;
    &.left{
        float: left;
        margin-right: 20px;
    }
    &.right{
        float: right;
        margin-left: 20px;
    }
}

.cropper{
    position: relative;
    width: 100%;
    overflow: hidden;
    margin-bottom: 5px;
    box-sizing: border-box;
}

.imgInfo{
    display: flex;
    flex-wrap: wrap;
    align-items: center;
}

.inp_w, .inp_h{
    width: 100px;
    min-width: 100px;
    height: 34px;
    box-sizing: border-box;
    padding: 0 10px;
    margin-left: 10px;
    outline: none;
}

.inp_h{
    margin-right: 10px;
}

.inp_alt{
    width: 100%;
    height: 34px;
    flex-grow: 1;
    box-sizing: border-box;
    padding: 0 10px;
    margin-top: 10px;
    outline: none; 
    &::-webkit-input-placeholder{opacity: 1; transition: opacity 0.3s ease;}
    &::-moz-placeholder{opacity: 1; transition: opacity 0.3s ease;}
    &:-moz-placeholder{opacity: 1; transition: opacity 0.3s ease;}
    &:-ms-input-placeholder{opacity: 1; transition: opacity 0.3s ease;}
    &:focus::-webkit-input-placeholder{opacity: 0; transition: opacity 0.3s ease;}
    &:focus::-moz-placeholder{opacity: 0; transition: opacity 0.3s ease;}
    &:focus:-moz-placeholder{opacity: 0; transition: opacity 0.3s ease;}
    &:focus:-ms-input-placeholder{opacity: 0; transition: opacity 0.3s ease;}
}
</style>