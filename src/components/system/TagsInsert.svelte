<script>
    import { createEventDispatcher } from 'svelte';

    const dispatch = createEventDispatcher();

    function addNodeClick(tag) {
        let contentNode;

        switch (tag) {
            case 'ul':
                contentNode = '<li>Пункт списка</li>';
                break;
            case 'ol':
                contentNode = '<li>Пункт списка</li>';
                break;
            case 'quote_1':
                contentNode = {
                    title: 'Заголовок', text: 'Текст'
                };
                break;
            case 'img_1':
                contentNode = {
                    url: '/tech/no-image-fh.jpg', 
                    alt: '', 
                    height: 450
                };
                break;
            case 'img_left':
                contentNode = {
                    url: '/tech/no-image.jpg', 
                    alt: '', 
                    width: 300,
                    height: 300,
                    position: true,
                    text: `
                        <p>Lorem ipsum dolor sit amet, consectetuer adipiscing elit. Aenean commodo ligula eget dolor. Aenean massa. 
                        Cum sociis natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. 
                        Donec quam felis, ultricies nec, pellentesque eu, pretium quis, sem.</p>
                        <p>Lorem ipsum dolor sit amet, consectetuer adipiscing elit. Aenean commodo ligula eget dolor. Aenean massa. 
                        Cum sociis natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus.</p>
                        <p>Lorem ipsum dolor sit amet, consectetuer adipiscing elit. Aenean commodo ligula eget dolor. Aenean massa. 
                        Cum sociis natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. 
                        Donec quam felis, ultricies nec, pellentesque eu, pretium quis, sem. Nulla consequat massa quis enim.</p>
                    `
                };
                break;
            default:
                contentNode = '';
        }
        
        dispatch('onSelectNode', {
            typeNode: tag,
            contentInner: contentNode
        });
    }

    let base = [
        {type: 'h1', title: "Заголовок"},
        {type: 'h2', title: "Заголовок"},
        {type: 'h3', title: "Заголовок"},
        {type: 'h4', title: "Заголовок"},
        {type: 'p', title: "Абзац"},
        {type: 'ul', title: "Список"},
        {type: 'ol', title: "Список"},
    ];

    // let code = [
    //     {type: 'code_html', title: "Код<br> HTML"},
    //     {type: 'code_css', title: "Код<br> CSS"},
    //     {type: 'noindex', title: "Пример<br> noindex"},
    // ];

    let quote = [
        {type: 'quote_1', title: "Цитата"},
    ];

    let image = [
        {type: 'img_1', title: "Изображение"},
        {type: 'img_left', title: "Фото & абзац"},
    ];
</script>

<div class="tags">
    {#each base as tag}
        <button class="tag {tag.type}" on:click={() => addNodeClick(tag.type)}>
            {tag.title}
        </button>
    {/each}
</div>

<!-- <div class="separator">Для формирования кода</div>

<div class="tags">
    {#each code as tag}
        <button class="tag {tag.type}" on:click={() => addNodeClick(tag.type)}>
            {@html tag.title}
        </button>
    {/each}
</div> -->

<div class="separator">Изображения</div>

<div class="tags">
    {#each image as tag}
        <button class="tag {tag.type}" on:click={() => addNodeClick(tag.type)}>
            {@html tag.title}
        </button>
    {/each}
</div>

<div class="separator">Цитаты</div>

<div class="tags">
    {#each quote as tag}
        <button class="tag {tag.type}" on:click={() => addNodeClick(tag.type)}>
            {@html tag.title}
        </button>
    {/each}
</div>

<style lang="scss">
.tags{
    display: flex;
    flex-wrap: wrap;
    max-width: 440px;
}

.tag{
    position: relative;
    width: 100px;
    margin: 5px;
    text-align: center;
    font-size: 14px;
    color: #000;
    border: 1px solid rgba(0,0,0,0.05);
    border-radius: 3px;
    box-sizing: border-box;
    padding: 60px 5px 5px 5px;
    cursor: pointer;
}

.tag:hover{
    border: 1px solid rgba(0,0,0,0.3); 
}

.tag:before{
    content: '';
    position: absolute;
    top: 5px;
    left: 50%;
    width: 50px;
    height: 50px;
    margin-left: -25px;
    background-position: center center;
    background-repeat: no-repeat;
    background-size: contain;
}

.separator{
    width: calc(100% - 10px);
    padding: 3px 10px;
    margin: 5px;
    box-sizing: border-box;
    background-color: #423232;
    color: #fff;
    text-align: center;
}

.tag.h1:before{
    background-image: url(/svg/editor/tag-h1.svg);
}

.tag.h2:before{
    background-image: url(/svg/editor/tag-h2.svg);
}

.tag.h3:before{
    background-image: url(/svg/editor/tag-h3.svg);
}

.tag.h4:before{
    background-image: url(/svg/editor/tag-h4.svg);
}

.tag.p:before{
    background-image: url(/svg/editor/tag-p.svg);
}

.tag.ul:before{
    background-image: url(/svg/editor/tag-ul.svg);
}

.tag.ol:before{
    background-image: url(/svg/editor/tag-ol.svg);
}

.tag.code_html:before{
    background-image: url(/svg/editor/tag-code_html.svg);
}

.tag.code_css:before{
    background-image: url(/svg/editor/tag-code_css.svg);
}

.tag.noindex:before{
    background-image: url(/svg/editor/tag-noindex.svg);
}

.tag.quote_1:before{
    background-image: url(/svg/editor/tag-quote.svg);
}

.tag.img_1:before{
    background-image: url(/svg/editor/tag-img.svg);
}

.tag.img_left:before{
    background-image: url(/svg/editor/tag-img-text.svg);
}
</style>