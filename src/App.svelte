<script lang="ts">
    import { onMount } from 'svelte';

    let gyro;
    let errMsg = '';

    onMount(() => {
        try {
            gyro = new Gyroscope({ frequency: 60 });
            gyro.addEventListener('reading', (e) => {
                console.log('Angular velocity along the X-axis ' + gyro.x);
                console.log('Angular velocity along the Y-axis ' + gyro.y);
                console.log('Angular velocity along the Z-axis ' + gyro.z);
            });
            gyro.start();
        } catch (error) {
            errMsg = error;
        }
    });
</script>

<div>
    <h3>gyro</h3>
    <span>{errMsg}</span>
    {#if gyro}
        <span>x:{gyro.x}</span><span>y:{gyro.y}</span><span>z:{gyro.z}</span>
    {/if}
</div>

<style lang="scss">
    div {
        display: flex;
        flex-direction: column;
    }
</style>
