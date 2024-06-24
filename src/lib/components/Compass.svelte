<script lang="ts">
    import { onMount, tick } from "svelte";

    let compass: HTMLImageElement;

    const handleMouseMouse = (e: MouseEvent) => {
        if (!compass) return;

        const clientX = e.clientX;
        const clientY = e.clientY;

        const { top, left, width, height } = compass.getBoundingClientRect();
        const compassX = left + width / 2;
        const compassY = top + height / 2;

        const angle = 90 + Math.atan2((clientY - compassY), (clientX - compassX)) * 180 / Math.PI;

        // Get the current rotation of the compass
        const computedStyle = window.getComputedStyle(compass);
        const transform = computedStyle.transform;

        let currentRotation = 0;
        if (transform !== 'none') {
            const values = transform.split('(')[1].split(')')[0].split(',');
            const a = parseFloat(values[0]);
            const b = parseFloat(values[1]);
            currentRotation = Math.atan2(b, a) * (180 / Math.PI);
        }

        let rotationDelta = angle - currentRotation;
        if (rotationDelta > 180) rotationDelta -= 360;
        if (rotationDelta < -180) rotationDelta += 360;
        const newRotation = currentRotation + rotationDelta;

        if (Math.abs(rotationDelta) > 30) {
            compass.animate([
                { transform: `rotate(${currentRotation}deg)` },
                { transform: `rotate(${newRotation + (rotationDelta < 0 ? -20 : 20)}deg)` },
                { transform: `rotate(${newRotation}deg)` }
            ], {
                duration: 500,
                easing: 'cubic-bezier(0.68, 0.55, 0.27, 1.55)',
                fill: 'forwards'
            });
        } else {
            compass.animate([
                { transform: `rotate(${currentRotation}deg)` },
                { transform: `rotate(${newRotation}deg)` }
            ], {
                duration: 10,
                easing: 'ease-in-out',
                fill: 'forwards'
            });
        }
    }

    onMount(async () => {
        await tick();
        if (!compass) return;

        compass.animate([
            { transform: 'rotate(0deg)'},
            { transform: `rotate(${360*2 + 20}deg)`},
            { transform: `rotate(${360*2}deg)`},
        ], {
            duration: 1000,
            easing: 'cubic-bezier(0.68, 0.55, 0.27, 1.55)'
        });

        await new Promise(r => setTimeout(r, 1000))

        if (window.matchMedia('(min-width:768px)').matches) {
            window.addEventListener('mousemove', handleMouseMouse);
            return;
        }

        let currentRotation = 0;
        const intervalId = setInterval(() => {
            const newAngle = Math.floor(Math.random() * 360);

            let rotationDelta = newAngle - currentRotation;
            if (rotationDelta > 180) rotationDelta -= 360;
            if (rotationDelta < -180) rotationDelta += 360;
            const newRotation = currentRotation + rotationDelta;

            compass.animate([
                { transform: `rotate(${currentRotation}deg)`},
                { transform: `rotate(${newRotation + (rotationDelta < 0 ? -20 : 20)}deg)`},
                { transform: `rotate(${newRotation}deg)`},
            ], {
                duration: 1000,
                easing: 'cubic-bezier(0.68, 0.55, 0.27, 1.55)',
                fill: 'forwards',
            });
            currentRotation = newAngle;
        }, 2000);
    });
</script>

<div class={`md:h-[125px] h-[0px] grid place-items-center`}>
    <button>
        <img bind:this={compass} src='/images/compass.svg' alt='compass' class={`md:h-[125px] h-[50px]`}/>
    </button>
</div>
