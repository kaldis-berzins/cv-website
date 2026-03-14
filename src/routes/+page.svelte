<script>
    import portrait from '$lib/assets/portrait.jpeg';
    import DarkMode from '@iconify-svelte/material-symbols/dark-mode-rounded';
    import LightMode from '@iconify-svelte/material-symbols/light-mode-rounded';
    import GitHub from '@iconify-svelte/mdi/github';
    import LinkedIn from '@iconify-svelte/mdi/linkedin';

    let circle;
    let darkMode = $state(false);
    let hovering = false;

    $effect(() => {       
        let previousX = 0;
        let previousY = 0;
        let mousedown = false;
        window.addEventListener("mousemove", (e) => {
            circle.style.setProperty("--x", `${e.clientX}px`);
            circle.style.setProperty("--y", `${e.clientY}px`);
            circle.style.setProperty("display", "block");
            
            if (previousX !== 0 && previousY !== 0 && !mousedown && !hovering) {
                let dx = Math.abs(previousX - e.clientX);
                let dy = Math.abs(previousY - e.clientY);
                let d = Math.sqrt(dx * dx + dy * dy);
                circle.style.setProperty("--size", `${Math.max(20 - d * 0.5, 0)}px`);
            }

            previousX = e.clientX;
            previousY = e.clientY;
        });

        window.addEventListener("mousedown", (e) => {
            if(hovering) circle.style.setProperty("--size", "85px");
            else circle.style.setProperty("--size", "25px");

            mousedown = true;
        });

        window.addEventListener("mouseup", (e) => {
            if(hovering) circle.style.setProperty("--size", "75px");
            else circle.style.setProperty("--size", "20px");
            mousedown = false;
        });

        if(localStorage.getItem("darkMode") === null) {
            darkMode = window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches;
        } else {
            darkMode = localStorage.getItem("darkMode") === "true";
        }
    });

    function onmouseenter() {
        if (circle) {
            circle.style.setProperty("--size", "75px");
            hovering = true;
        }
    }

    function onmouseleave() {
        if (circle) {
            circle.style.setProperty("--size", "20px");
            hovering = false;
        }
    }

    function toggleDarkMode() {
        darkMode = !darkMode;
        localStorage.setItem("darkMode", darkMode);
    }
</script>
<div id="app" class="{darkMode ? 'dark-mode mesh-background' : ''}">
    <nav>
        <a href="/" class="home" {onmouseenter} {onmouseleave}>K.K.B.</a>
        <button id="dark-mode-toggle" {onmouseenter} {onmouseleave} onclick={toggleDarkMode}>
            {#if darkMode}
                 <LightMode class="icon" width="32px" height="32px"/>
            {:else}
                <DarkMode class="icon" width="32px" height="32px"/>
            {/if}
        </button>
    </nav>
    <div id="content">
        <img src="{portrait}" alt="Portrait of Kaldis" id="portrait">
        <h1>Kaldis Kariņš Bērziņš</h1>
        <div id="links-container">
            <a href="https://www.linkedin.com/in/kaldis-berzins-600b2a2a7/" class="link" {onmouseenter} {onmouseleave}><LinkedIn class="icon"/>LinkedIn</a>
            <a href="https://github.com/kaldis-berzins" class="link" {onmouseenter} {onmouseleave}><GitHub class="icon"/>GitHub</a>
        </div>
        
        <div id="cursor-circle" bind:this="{circle}" style="display: none"><div class="mesh-background cursor-background"></div></div>
    </div>
</div>




<style lang="scss">
    :root {
        --text: #000000;
        --background: #ffffff;
    }

    .dark-mode {
        --text: #ffffff;
        --background: #000000;
        filter: none;
    }

    :global(.icon) {
        color: var(--text);
        width: 1em;
        height: 1em;
    }

    #app {
        background-color: var(--background);
        color: var(--text);
        width: 100%;
        min-height: 100vh;
        nav {
            padding-left: 32px;
            padding-right: 32px;
            padding-top: 16px;
            padding-bottom: 16px;
            display: flex;
            .home {
                font-family: 'Noticia Text';
                font-size: 48px;
                font-weight: bold;
                text-decoration: none;
                color: var(--text);
            }

            #dark-mode-toggle {
                margin-left: auto;
                font-size: 48px;
                background: none;
                border: none;
                cursor: pointer;
                color: var(--text);
            }
        }

        #content {
            display: flex;
            flex-direction: column;
            align-items: center;
            padding-top: 64px;
            padding-bottom: 64px;
            #portrait {
                border-radius: 32px;
                position: relative;
                z-index: 10;
            }

            #links-container {
                display: flex;
                flex-direction: row;
                gap: 48px;

                .link {
                    display: flex;
                    flex-direction: column;
                    align-items: center;
                    gap: 8px;
                    color: var(--text);
                    text-decoration: none;

                    &:hover {
                        text-decoration: underline;
                    }
                }

                :global(.link .icon) {
                    width: 96px;
                    height: 96px;
                }
            }
        }
    }

    h1 {
        font-family: 'Noticia Text';
        font-size: 64px;
        font-weight: bold;
        text-align: center;
    }

    @media (max-width: 768px) {
        h1 {
            font-size: 48px;
        }

        #cursor-circle {
            opacity: 0;
        }
    }
    
    #cursor-circle {
        clip-path: circle(var(--size) at var(--x) var(--y));
        transition: clip-path 0.05s linear;
        position: fixed;
        width: 100vw;
        height: 100vh;
        top:0;
        left: 0;
        pointer-events: none;
        mix-blend-mode: difference;

        .cursor-background {
            width: 100%;
            height: 100%;
        }
    }
</style>