<script lang='ts'>

</script>

<div class="wrap wrap_why">
	<div class="work">
        <h2 class="w_title">Почему svelte.js?</h2>
        <div class="why">
            <div class="block">
                <div class="icon speed"></div>
                <div class="title">Скорость<br> разработки</div>
                <div class="text">
                    Типичный компонент на Svelte в 3 раза меньше, чем полный аналог на Vue, и в 5 раз меньше чем на React.
                </div>
            </div>
            <div class="block">
                <div class="icon dom"></div>
                <div class="title">Компиляция в обычные <br> DOM-операции</div>
                <div class="text">
                    У Svelte нет никаких Virtual DOM: после сборки компонент работает с нативным DOM браузера!
                </div>
            </div>
            <div class="block">
                <div class="icon runtime"></div>
                <div class="title">Минималистичный <br> рантайм</div>
                <div class="text">
                    После компиляции Svelte в бандле от него практически ничего не остается – только непосредственно логика приложения и функционал, 
                    который используется напрямую.
                </div>
            </div>
            <div class="block">
                <div class="icon performance"></div>
                <div class="title">Хорошая <br> производительность</div>
                <div class="text">
                    React и Vue тратят ресурсы пользователя на постоянный пересчет virtual DOM. Jquery работает с настоящим DOM, но меняет сразу много 
                    DOM узлов. А Svelte делает точечные изменеения в DOM.
                </div>
            </div>
            <div class="block">
                <div class="icon typescript"></div>
                <div class="title">Хорошая поддержка <br> TypeScript</div>
                <div class="text">
                    Svelte позволяет делать проверку типов не только в коде, но и в шаблоне, что позволяет отлавливать больше ошибок на этапе разработки.
                </div>
            </div>
            <div class="block">
                <div class="icon tools"></div>
                <div class="title">Инструменты из <br> коробки</div>
                <div class="text">
                    В Svelte из коробки есть инструменты управления состоянием (store), анимации, экшены, асинхронная отрисовка (await в шаблоне).
                </div>
            </div>
        </div>
    </div>
</div>

<style lang="scss">
.wrap_why{
    padding: 50px 0;
    background-color: #F0F0F1;
}

.w_title{
    width: 100%;
    text-align: center;
    font-weight: 500;
    font-size: 32px;
}

.why{
    display: flex;
    flex-wrap: wrap;
    width: 100%;
    margin-top: 25px;
}

.block{
    width: calc(100%/3 - 30px);
    margin: 15px;
    background-color: #fff;
    border-radius: 5px;
    box-sizing: border-box;
    padding: 15px;
}

.icon{
    width: 50px;
    height: 50px;
    background-position: left bottom;
    background-repeat: no-repeat;
    background-size: contain;
    &.speed{
        background-image: url(/svg/why_svelte/speed.svg);
    }
    &.performance{
        background-image: url(/svg/why_svelte/performance.svg);
        background-size: 90% auto;
    }
    &.typescript{
        background-image: url(/svg/why_svelte/typescript.svg);
    }
    &.runtime{
        background-image: url(/svg/why_svelte/runtime.svg);
    }
    &.tools{
        background-image: url(/svg/why_svelte/tools.svg);
        background-size: 85% auto;
    }
    &.dom{
        background-image: url(/svg/why_svelte/dom.svg);
        background-size: 85% auto;
    }
}

.title{
    font-size: 20px;
    color: #252525;
    font-weight: 500;
    margin-top: 10px;
    line-height: 1.3em;
}

.text{
    width: 100%;
    margin-top: 15px;
    line-height: 1.6em;
    font-size: 14px;
    color: #656565;
}

@media only screen and (max-width: 767px){
    .block{
        width: calc(100%/2 - 30px);
    }
}

@media only screen and (max-width: 539px){
    .block{
        width: 100%;
        margin: 15px 0;
    }
}
</style>