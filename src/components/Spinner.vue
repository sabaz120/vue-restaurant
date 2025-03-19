<template>
    <div>
        <svg class="pl" viewBox="0 0 48 48" width="48px" height="48px" role="img"
            aria-label="A dot attempts to catch up to one on the right by moving like a Slinky, but it keeps inching away">
            <defs>
                <linearGradient id="pl-grad" x1="0" y1="0" x2="1" y2="1">
                    <stop offset="0%" stop-color="#000" />
                    <stop offset="100%" stop-color="#fff" />
                </linearGradient>
                <mask id="pl-mask">
                    <!-- 1px bleed to help reduce gradient cropping glitch (size is actually 48×16) -->
                    <rect x="-1" y="-1" width="50" height="18" fill="url(#pl-grad)" />
                </mask>
            </defs>
            <g fill="none" stroke="currentcolor" stroke-linecap="round" stroke-width="2" transform="translate(0,19)">
                <g class="pl__layer">
                    <g class="pl__scene">
                        <path class="pl__curve" d="M 16 9 C 16 4.582 19.582 1 24 1 C 28.418 1 32 4.582 32 9"
                            stroke-dasharray="25.13 25.13" stroke-dashoffset="25.12" />
                        <polyline class="pl__dot" points="32 9,48 9" stroke-dasharray="0.01 16" />
                    </g>
                </g>
                <g class="pl__layer" mask="url(#pl-mask)">
                    <g class="pl__scene">
                        <path class="pl__curve" d="M 16 9 C 16 4.582 19.582 1 24 1 C 28.418 1 32 4.582 32 9"
                            stroke-dasharray="25.13 25.13" stroke-dashoffset="25.12" />
                        <polyline class="pl__dot" points="32 9,48 9" stroke-dasharray="0.01 16" />
                    </g>
                </g>
            </g>
        </svg>
    </div>
</template>

<script>
export default {
    name: 'Spinner'
}
</script>

<style lang="scss" scoped>
* {
    border: 0;
    box-sizing: border-box;
    margin: 0;
    padding: 0;
}

:root {
    --hue: 223;
    --sat: 90%;
    --primary1: hsl(var(--hue), var(--sat), 90%);
    --primary5: hsl(var(--hue), var(--sat), 50%);
    --primary9: hsl(var(--hue), var(--sat), 10%);
    --red: hsl(343, 90%, 50%);
    --teal: hsl(163, 90%, 50%);
    --purple: hsl(283, 90%, 50%);
    --trans-dur: 0.3s;
    color-scheme: light dark;
    font-size: clamp(1rem, 0.9rem + 0.5vw, 1.5rem);
}

body {
    background-color: light-dark(var(--primary1), var(--primary9));
    color: light-dark(var(--primary9), var(--primary1));
    display: flex;
    font: 1em/1.5 sans-serif;
    height: 100vh;
    transition:
        background-color var(--trans-dur),
        color var(--trans-dur);
}

.pl {
    display: block;
    margin: auto;
    width: 16em;
    height: auto;

    &__curve,
    &__dot,
    &__scene {
        animation: {
            duration: 1s;
            timing-function: cubic-bezier(0.65, 0, 0.35, 1);
            iteration-count: infinite;
        }

        ;
    }

    &__curve,
    &__dot {
        transition: stroke var(--trans-dur);
    }

    &__curve {
        animation-name: curve-offset;
        stroke: var(--primary5);
    }

    &__layer+&__layer &__curve {
        stroke: var(--teal);
    }

    &__dot {
        animation-name: dot-move;
        stroke: var(--red);
    }

    &__layer+&__layer &__dot {
        stroke: var(--purple);
    }

    &__scene {
        animation: {
            name: scene-move;
            timing-function: linear;
        }

        ;
    }
}

/* Animations */
@keyframes curve-offset {
    from {
        stroke-dashoffset: 25.12;
    }

    46% {
        stroke-dashoffset: 0;
    }

    92%,
    to {
        stroke-dashoffset: -24.97;
    }
}

@keyframes dot-move {

    from,
    25% {
        stroke-dashoffset: 0;
    }

    50%,
    to {
        stroke-dashoffset: -15.99;
    }
}

@keyframes scene-move {
    from {
        transform: translate(0, 0);
    }

    to {
        transform: translate(-16px, 0);
    }
}
</style>