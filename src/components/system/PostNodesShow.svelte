<script lang='ts'>
    import type {Nodes} from '../../routes/types';
    import Highlight from "svelte-highlight";
	import vbscriptHtml from "svelte-highlight/languages/vbscript-html";
	import css from "svelte-highlight/languages/css";
    
    import QuoteOne from './QuoteOne.svelte';

    export let nodes: Nodes;
</script>


{#each nodes as node, index}
    {#if node.type === 'h1'}
        <h1>{@html node.content}</h1>
    {/if}
    {#if node.type === 'h2'}
        <h2>{@html node.content}</h2>
    {/if}
    {#if node.type === 'h3'}
        <h3>{@html node.content}</h3>
    {/if}
    {#if node.type === 'h4'}
        <h4>{@html node.content}</h4>
    {/if}
    {#if node.type === 'p'}
        <p>{@html node.content}</p>
    {/if}
    {#if node.type === 'ul'}
        <ul>{@html node.content}</ul>
    {/if}
    {#if node.type === 'ol'}
        <ol>{@html node.content}</ol>
    {/if}
    {#if node.type === 'code_html'}
        <Highlight language={vbscriptHtml} code={node.content} />
    {/if}
    {#if node.type === 'code_css'}
        <Highlight language={css} code={node.content} />
    {/if}
    {#if node.type === 'noindex'}
        <noindex>{@html node.content}</noindex>
    {/if}
    {#if node.type === 'quote_1'}
        <QuoteOne title={node.content.title} text={node.content.text}/>
    {/if}
    {#if node.type === 'img_1'}
        <img class="single_image" src="{node.content.url}" alt="{node.content.alt}"  />
    {/if}
    {#if node.type === 'img_left'}
        <img 
            class="image_text {node.content.position ? 'left' : 'right'}" 
            src="{node.content.url}" 
            alt="{node.content.alt}" 
            width="{node.content.width}" 
            height="{node.content.height}"  
        />
        {@html node.content.text}
    {/if}
{/each}

<style lang="scss">
h1{
    font-size: 24px;
    font-weight: 500;
}

h2{
  font-size: 20px;
  font-weight: 500;
}

h3{
  font-size: 18px;
  font-weight: 500;
}

h4{
  font-size: 16px;
  font-weight: 500;
}

p{
  font-size: 16px;
  line-height: 1.7em;
}

:global(p){
    font-size: 16px;
    line-height: 1.7em;
}

ul{
  list-style-type: none;
  padding: 0;
  margin: 0;
  margin-bottom: 20px;
}

ul :global(li){
    position: relative;
    font-size: 16px;
    line-height: 1.4em;
    box-sizing: border-box;
    padding-left: 35px;
    margin-bottom: 10px;
    &::before{
        content: '';
        position: absolute;
        top: 4px;
        left: 10px;
        width: 16px;
        height: 16px;
        background-image: url(/svg/check.svg);
        background-repeat: no-repeat;
        background-position: center center;
        background-size: contain;
    }
}

.single_image{
    width: 100%;
    max-width: 100%;
    vertical-align: bottom;
    border: 1px solid rgba(0,0,0,0.2);
    padding: 5px;
}

.image_text{
    max-width: 100%;
    vertical-align: bottom;
    border: 1px solid rgba(0,0,0,0.2);
    padding: 5px;
    &.left{
        float: left;
        margin-right: 20px;
    }
    &.right{
        float: right;
        margin-left: 20px;
    }
}

@media only screen and (max-width: 539px){
    .image_text{
        &.left{
            float: inherit;
            margin-right: 0;
        }
        &.right{
            float: inherit;
            margin-left: 0;
        }
    }
}
</style>