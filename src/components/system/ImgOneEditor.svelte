<script lang='ts'>
    import Cropper from "svelte-easy-crop";
    import { cropImage } from '../../domain';
    import ButtonUploadFile from './ButtonUploadFile.svelte';
    import Button from './Button.svelte';

    export let image:string;
    export let altImage:number;
    export let hImage:number;

    import { uploadImage } from '../../requests';
    


    let cropData:Object;
    $: cropSize = {width: 800, height: hImage};

    let isUploadImage:boolean;

    async function saveImage(){
        if (isUploadImage){
			const blob = await cropImage(image, cropData, cropSize, 'image/jpeg', 1);
			let res = await uploadImage('image/jpeg', blob);
			image = res.url;
		}
    }
</script>

<div class="container">
    <div class="image">
        <div class="cropper">
            <Cropper 
                image={image}
                crop={{ x: 0, y: 0 }}
                zoom={1}
                minZoom={0.8}
                maxZoom={2}
                aspect = {800/hImage}
                zoomSpeed={0.1}
                restrictPosition={false}
                on:cropcomplete={(event) => cropData = event.detail.pixels}
            />
        </div>
        <div class="imgInfo">
            <ButtonUploadFile bind:imgFile={image} bind:imgUpload={isUploadImage} />
            <input class="inp_h" type="number" bind:value={hImage} />
            <input class="inp_alt" type="text" bind:value={altImage}  />
            <Button title="Сохранить" on:click="{saveImage}" />
        </div>
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

.image{
    width: 100%;
    background-color: #fff;
    border-top: 1px solid rgba(10, 10, 10, 0.1);
    box-sizing: border-box;
    padding: 10px;
}

.cropper{
    position: relative;
    width: 100%;
    height: 500px;
    overflow: hidden;
    margin-bottom: 5px;
    box-sizing: border-box;
}

.imgInfo{
    display: flex;
}

.inp_h{
    width: 75px;
    min-width: 75px;
    box-sizing: border-box;
    padding: 0 10px;
    margin-left: 10px;
    outline: none;
}

.inp_alt{
    flex-grow: 1;
    box-sizing: border-box;
    padding: 0 10px;
    margin-left: 10px;
    margin-right: 10px;
    outline: none; 
}
</style>