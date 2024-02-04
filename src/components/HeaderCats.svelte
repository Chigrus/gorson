<script lang='ts'>

</script>

<div class="cats">
    <a href="/" class="cat">
        <div class="title">Технологии</div>
    </a>
    <a href="/gadgets" class="cat">
        <div class="title">Гаджеты</div>
    </a>
    <a href="/" class="cat">
        <div class="title">Расчеты</div>
    </a>
    <a href="/" class="cat">
        <div class="title">Сад</div>
    </a>
    <a href="/garage" class="cat">
        <div class="title">Гараж</div>
    </a>
    <a href="/" class="cat">
        <div class="title">Сделай сам</div>
    </a>
    <a href="/" class="cat">
        <div class="title">Строительство</div>
    </a>
</div>

<style lang="scss">
.cats{
    display: grid;
    grid-template-columns: 1fr 210px 210px 210px;
    grid-template-rows: 210px 210px;
    grid-column-gap: 10px;
    grid-row-gap: 10px;
}

.cat{
    position: relative;
    display: flex;
    align-items: self-end;
    padding: 20px;
    background-color: rgba(0,0,0,0.1);
    text-decoration: none;
    background-position: center center;
    background-repeat: no-repeat;
    background-size: cover;
    &::before{
        content: '';
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background-color: rgba(0,0,0,0.4);
        transition: .5s;
    }
    &:nth-child(1){
        grid-row-start: 1;
        grid-row-end: 3;
        background-image: url(/img/home_1.jpg);
    }
    &:nth-child(2){
        background-image: url(/img/home_2.jpg);
    }
    &:nth-child(3){
        background-image: url(/img/home_3.jpg);
    }
    &:nth-child(4){
        background-image: url(/img/home_4.jpg);
    }
    &:nth-child(5){
        background-image: url(/img/home_5.jpg);
    }
    &:nth-child(6){
        background-image: url(/img/home_6.jpg);
    }
    &:nth-child(7){
        background-image: url(/img/home_7.jpg);
    }
}

.cat:hover{
    &::before{
        background-color: rgba(0,0,0,0.2);
        transition: .5s;
    }
}

.title{
    position: relative;
    z-index: 1;
    background-color: rgba(0,0,0,1);
    border-radius: 3px;
    color: #fff;
    font-size: 14px;
    padding: 5px 10px;
    line-height: 1em;
}


@media only screen and (max-width: 1023px){
    .cats{
        grid-template-columns: 1fr 1fr 1fr 1fr;
        grid-template-rows: 210px 210px 210px;
    }
	.cat{
		&:nth-child(1){
            grid-column-start: 1;      
            grid-column-end: 3;      
        }
	}	
}

@media only screen and (max-width: 899px){
    .cats{
        grid-template-rows: 170px 170px 170px;
    }	
}

@media only screen and (max-width: 767px){
    .cats{
        grid-template-columns: 1fr 1fr;
        grid-template-rows: 210px 210px 210px 210px 210px;
    }	
}

@media only screen and (max-width: 479px){
    .cats{
        grid-template-columns: 1fr 1fr;
        grid-template-rows: 180px 180px 180px 180px 180px;
        grid-column-gap: 5px;
        grid-row-gap: 5px
    }	
}
</style>