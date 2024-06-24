<script lang="ts">
    import Compass from "$lib/components/Compass.svelte";
    import { onMount, tick } from "svelte";
    import { fly } from "svelte/transition";
    import { elasticOut } from "svelte/easing";

    let container: HTMLElement;
    let title: HTMLDivElement;
    let descOne: HTMLDivElement;
    let displayHomeImg: boolean = true;

    const handleScroll = (e: Event) => {
        const scrollTop = container.scrollTop;
        if (title && descOne) {
            if (scrollTop <= (title.clientHeight + descOne.clientHeight/2)) {
                displayHomeImg = true;
            } else {
                displayHomeImg = false;
            }
        }
    }

    let ready: boolean = false;

    onMount(async () => {
        await tick();
        ready = true;
        if (container) {
            container.addEventListener('scroll', handleScroll)

            const observer = new IntersectionObserver((entries) => {
                entries.forEach((entry) => {
                    if (entry.isIntersecting) {
                        entry.target.classList.add('show');
                    }
                })
            });

            if (window.matchMedia('(min-width:768px)').matches) {
                const hiddenElements = document.querySelectorAll('.hide');
                hiddenElements.forEach((el) => observer.observe(el));
            }
        }
    })

</script>

<main bind:this={container} class={`container`}>
    <section class={`title-page`}>
        <div bind:this={title} class={`h-screen w-screen relative grid place-items-center`}>
            <div class={`absolute h-full w-full bg-dots fade-m -z-10`}></div>

            {#if (ready)}
                <div class={`w-fit md:pr-0 pr-2`}>
                    <div class={`flex md:gap-20 justify-end gap-8 pb-2`}>
                        <Compass />
                        <h1 class={`md:text-9xl text-5xl montserrat-500`} in:fly={{ duration: 1000, y: 40, easing: elasticOut }}>
                            mesh<span class={`text-purple-500`}>.nav</span>
                        </h1>
                    </div>
                    <p in:fly={{ delay: 0, duration: 1000, y: 20, easing: elasticOut }} class={`md:text-5xl text-lg md:mt-4 text-right`}>
                        <span>effortless navigation.</span>
                        <span>personalised deals.</span>
                    </p>
                    <p in:fly={{ delay: 0, duration: 1000, y: 20, easing: elasticOut }} class={`md:text-3xl text-sm md:mt-2 text-neutral-600 text-right`}>
                        the future of the personalised retail store experience.
                    </p>
                </div>
            {/if}
        </div>
    </section>

    <section class={`display-page`}>
        <div class={`relative h-fit w-screen flex justify-center items-center`}>
            <div class={`absolute h-full w-full bg-dots fade-m -z-10`}></div>
            <div class={`md:static showcase-container absolute`}>
                <div class={`showcase-div`}>
                    <img src={`/images/${displayHomeImg ? 'homepage' : 'nav'}.png`} alt="mockup" class={`showcase-img drop-shadow-2xl shadow-purple-600 shadow-2xl bg-purple-600 rounded-[45px]`}/>
                </div>
            </div>

            <div  class={`w-full md:w-[600px] md:ml-[100px] z-10`}>
                <div bind:this={descOne} class={`descriptor h-screen flex flex-col justify-center items-center`}>
                    <div class={`hide bg-zinc-950/90 md:bg-transparent backdrop-blur-sm md:backdrop-blur-none px-4 py-4 rounded-xl md:border-0 border border-indigo-400 mx-2`}>
                        <h2 class={`text-purple-500 md:text-indigo-400 montserrat-500 text-4xl md:text-6xl text-center md:text-left`}>see where you want to be</h2>
                        <p class={`text-sm md:text-xl text-center md:text-left mt-2`}>Get personalised recommendations, find the top discounts and see where your friends are. Have your favourite malls at the tip of your fingertips with mesh.nav</p>
                    </div>
                </div>

                <div class={`descriptor h-screen flex flex-col justify-center items-center`}>
                    <div class={`hide bg-zinc-950/90 md:bg-transparent backdrop-blur-sm md:backdrop-blur-none px-4 py-4 rounded-xl md:border-0 border border-indigo-400 mx-2`}>
                        <h2 class={`text-purple-500 md:text-indigo-400 montserrat-500 text-4xl md:text-6xl text-center md:text-left`}>get where you need to be</h2>
                        <p class={`text-sm md:text-xl text-center md:text-left mt-2`}>With mesh.nav, navigation is as simple as following an arrow. That's it. Get wherever you need to be instantly, with the power of our next gen navigation engine</p>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <section class={`contact-page `}>
        <div class={`relative w-screen flex flex-col justify-center items-center border-b border-neutral-700 md:pt-10 pb-10`}>
            <div class={`absolute w-full h-full -z-10 bg-dots fade-m`}></div>

            <div class={`contact-card w-[80%] h-[90%] rounded-xl flex md:flex-row flex-col`}>
                <div class={`md:w-2/3 w-full px-4 py-4 flex flex-col justify-center`}>
                    <h3 class={`md:text-6xl text-3xl`}>sounds interesting?</h3>
                    <p class={`md:text-2xl text-md mt-2 text-neutral-300`}>We'd love to have you onboard. Sign up now to join the early access waitlist and get exclusive benefits at your favourite malls with mesh.nav</p>

                    <div class={`flex mt-4`}>
                        <input
                            class={`bg-transparent border border-neutral-50 rounded-md md:h-[50px] h-[40px] md:w-[500px] w-[200px] md:text-xl text-sm px-2`}
                            placeholder="Enter email address"
                        />
                        <button class={`bg-neutral-50 hover:opacity-85 active:opacity-80 rounded-md ml-2 md:h-[50px] h-[40px] md:px-8 px-4 text-xl text-zinc-950 montserrat-400`}>Join</button>
                    </div>
                </div>

                <div class={`md:w-1/3 w-full flex md:justify-end justify-center px-2 py-2`}>
                    <div class={`bg-neutral-800 px-8 py-2 flex flex-col justify-center items-center rounded-xl`}>
                        <img src='/logo.png' alt='logo' class={`h-[200px]`}/>
                        <p class={`text-3xl montserrat-500 md:mt-10 mt-2`}>mesh<span class={`text-purple-500`}>.nav</span></p>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <section class={`footer`}>
        <div class={`h-[80px] md:pl-10 pl-2 py-2`}>
            <h3 class={`montserrat-500`}>mesh.nav</h3>
            <p class={`text-sm text-neutral-400`}>The future of the personalised retail store experience.</p>
            <p class={`text-sm text-neutral-400`}>© 2024 MeshNav • All rights reserved • <a href="/">Privacy Policy</a></p>
        </div>
    </section>

</main>

<style>




    .container {
        scroll-snap-type: y mandatory;
        overflow-y: scroll;
        overflow-x: hidden;
        height: 100vh;
    }

    section {
        scroll-snap-align: start;
    }

    .descriptor {
        scroll-snap-align: start;
    }

    .showcase-div {
        position: relative;
        height: calc(200vh - ((100vh - 600px)));
    }

    .showcase-img {
        position: sticky;
        top: calc((100vh - 600px) / 2);
        height: 600px;
        width: 300px;
    }

    @media (min-width:768px) {
        .showcase-div {
            position: relative;
            height: calc(200vh - ((100vh - 600px)));
        }

        .showcase-img {
            position: sticky;
            top: calc((100vh - 600px) / 2);
            height: 600px;
            width: 300px;
        }
    }

    .contact-card {
        background-image: linear-gradient(
            90deg,
            #A201F4,
            #D201F8,
            #A201F4
        );
        background-size: 200%;
        animation: pan 10s linear infinite;
    }

    @keyframes pan {
        0% {
            background-position: 0% center;
        }
        100% {
            background-position: -200% center;
        }
    }
</style>