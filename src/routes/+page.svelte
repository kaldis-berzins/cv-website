<script>
    import blur from '$lib/assets/blur2.jpg';
    import portrait from '$lib/assets/portrait.jpeg';

    import github from '$lib/assets/github-mark.png';
    import linkedin from '$lib/assets/linkedin.png';

    let circle;
    $effect(() => {       
        let previousX = 0;
        let previousY = 0;
        let mousedown = false;

        window.addEventListener("mousemove", (e) => {
            circle.style.setProperty("--x", `${e.clientX}px`);
            circle.style.setProperty("--y", `${e.clientY}px`);
            circle.style.setProperty("display", "block");
            
            if (previousX !== 0 && previousY !== 0 && !mousedown) {
                let dx = Math.abs(previousX - e.clientX);
                let dy = Math.abs(previousY - e.clientY);
                let d = Math.sqrt(dx * dx + dy * dy);
                circle.style.setProperty("--size", `${Math.max(20 - d * 0.5, 0)}px`);
            }

            previousX = e.clientX;
            previousY = e.clientY;
        });

        window.addEventListener("mousedown", (e) => {
            circle.style.setProperty("--size", "25px");
            mousedown = true;
        });

        window.addEventListener("mouseup", (e) => {
            circle.style.setProperty("--size", "20px");
            mousedown = false;
        });
    });
    
</script>
<div id="app">
    <nav>
        <div class="home"><a href="/">K.K.B.</a></div>
    </nav>
    <div id="content">
        <img src="{portrait}" alt="Portrait of Kaldis" id="portrait">
        <h1>Kaldis Kariņš Bērziņš</h1>
        <div id="links-container">
            <a href="https://www.linkedin.com/in/kaldis-berzins-600b2a2a7/" class="link"><img src="{linkedin}" alt="LinkedIn">LinkedIn</a>
            <a href="https://github.com/kaldis-berzins" class="link"><img src="{github}" alt="GitHub">GitHub</a>
            
            <a href="https://www.linkedin.com/in/kaldis-berzins-600b2a2a7/" class="link">
                <span class="material-symbols-outlined">
                    file_open
                </span>
                Resume
            </a>
        </div>
        
        <div id="cursor-circle" bind:this="{circle}" style="display: none"><img alt="" src="{blur}"/></div>
    </div>
</div>




<style lang="scss">
    #app {
        background-color: white;
        width: 100%;
        min-height: 100vh;
        nav {
            padding-left: 48px;
            padding-top: 16px;
            padding-bottom: 16px;
            .home {
                a {
                    font-size: 48px;
                    font-weight: bold;
                    text-decoration: none;
                    color: black;
                    font-family: 'Noticia Text';
                    user-select: none;
                }
            }
        }
        height: 100%;
        #content {
            display: flex;
            flex-direction: column;
            align-items: center;
            padding-top: 64px;
            #portrait {
                border-radius: 32px;
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
                    color: black;
                    text-decoration: none;

                    &:hover {
                        text-decoration: underline;
                    }

                    .material-symbols-outlined {
                        font-size: 64px;
                        &:hover {
                            text-decoration: none;
                        }
                    }

                    img {
                        max-height: 64px;
                    }
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
    }
</style>