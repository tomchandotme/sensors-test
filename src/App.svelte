<script lang="ts">
    import { onMount } from 'svelte';

    let errMsg = '';

    let g = { x: 0, y: 0, z: 0 };

    const handleOrientation = (e: DeviceOrientationEvent) => {
        g.z = e.alpha;
        g.x = e.beta;
        g.y = e.gamma;
    };

    onMount(() => {
        try {
            window.addEventListener('deviceorientation', handleOrientation);
        } catch (error) {
            errMsg = error;
        }
    });
</script>

<div>
    <h3>gyro</h3>
    <span>{errMsg}</span>
    <span>x:{g.x}</span><span>y:{g.y}</span><span>z:{g.z}</span>
</div>

<style lang="scss">
    div {
        display: flex;
        flex-direction: column;
    }
</style>
