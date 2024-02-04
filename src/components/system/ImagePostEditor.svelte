<script lang='ts'>
    import Popup from './Popup.svelte';
    import Button from './Button.svelte';
    import Cropper from "svelte-easy-crop";
    import ButtonUploadFile from './ButtonUploadFile.svelte';

    import { cropImage } from '../../domain';
    import { uploadImage, saveIconPost } from '../../requests';

    export let imagePost:string;
    export let idPost:number;
    export let isImagePostEdit = false;
    export let widthImage:number;
    export let heightImage:number;
    export let imageField:string;

    let cropData:Object;
    let cropSize = {width: widthImage, height: heightImage};

    let isUploadImage:boolean;

    async function iconPostSave(){
        if (isUploadImage){
			const blob = await cropImage(imagePost, cropData, cropSize, 'image/jpeg', 1);
			let res = await uploadImage('image/jpeg', blob);
			imagePost = res.url;
            //console.log(imagePost);
		}
        saveIconPost(idPost, imagePost, imageField);
		isImagePostEdit = false;
    }
</script>

<Popup bind:isOpen="{isImagePostEdit}">
	<div slot="title">Редактировать иконку поста</div>
    <div slot="content">
        <div class="cropper">
            <Cropper 
                image={imagePost}
                crop={{ x: 0, y: 0 }}
                zoom={1}
                minZoom={0.8}
                maxZoom={2}
                aspect = {widthImage/heightImage}
                zoomSpeed={0.1}
                restrictPosition={false}
                on:cropcomplete={(event) => cropData = event.detail.pixels}
            />
        </div>
        <div class="imgInfo">
            <ButtonUploadFile bind:imgFile={imagePost} bind:imgUpload={isUploadImage} />
        </div>
    </div>
    <div slot="bottom">
		<div class="lineSave">
			<Button title="Сохранить" on:click="{iconPostSave}" />
		</div>
	</div>
</Popup>

<style lang="scss">
    .cropper{
        position: relative;
        float: left;
        width: 100%;
        height: 300px;
        overflow: hidden;
        margin-bottom: 5px;
        box-sizing: border-box;
    }
</style>