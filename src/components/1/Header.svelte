<script lang='ts'>
	import Nav from "../components/Nav.svelte";

    import { afterNavigate } from '$app/navigation';

    import { page } from '$app/stores';

    afterNavigate(() => {
        isMobileMenu = false; 
    });

    let isMobileMenu:boolean = false;

    function mobileClick(){
        isMobileMenu = true;
    }
</script>

<!-- <div class="header {$page.url.pathname !== '/' ? '' : 'home'}">
    <div class="line">
        <a href="/" class="logo">VERSTACHE.</a>
        <div class="emptyFlex"></div>
        <Nav bind:isOpen={isMobileMenu} />
    </div>
</div> -->

<div class="wrap">
    <div class="work">
        <header>
            11111111
        </header>
    </div>
</div>

<style lang="scss">

</style>