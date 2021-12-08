<script lang="ts">
    let isStarted = false;
    let accelerometer: {
        gx: number;
        gy: number;
        gz: number;
        x: number;
        y: number;
        z: number;
        i: number;
    } = { gx: 0, gy: 0, gz: 0, x: 0, y: 0, z: 0, i: 0 };

    let gyroscope: { x: number; y: number; z: number } = { x: 0, y: 0, z: 0 };

    let orientation: { x: number; y: number; z: number } = { x: 0, y: 0, z: 0 };

    const handleMotion = (e: DeviceMotionEvent) => {
        accelerometer.gx = e.accelerationIncludingGravity.x;
        accelerometer.gy = e.accelerationIncludingGravity.y;
        accelerometer.gz = e.accelerationIncludingGravity.z;

        accelerometer.x = e.acceleration.x;
        accelerometer.y = e.acceleration.y;
        accelerometer.z = e.acceleration.z;

        gyroscope.x = e.rotationRate.beta;
        gyroscope.y = e.rotationRate.gamma;
        gyroscope.z = e.rotationRate.alpha;

        accelerometer.i = e.interval;
    };

    const handleOrientation = (e: DeviceOrientationEvent) => {
        orientation.x = e.beta;
        orientation.y = e.gamma;
        orientation.z = e.alpha;
    };

    const start = (e: MouseEvent) => {
        e.preventDefault();

        if (
            DeviceMotionEvent &&
            typeof DeviceMotionEvent.requestPermission === 'function'
        ) {
            DeviceMotionEvent.requestPermission();
        }

        if (!isStarted) {
            window.addEventListener('devicemotion', handleMotion);
            window.addEventListener('deviceorientation', handleOrientation);
        } else {
            window.removeEventListener('devicemotion', handleMotion);
            window.removeEventListener('deviceorientation', handleOrientation);
        }

        isStarted = !isStarted;
    };
</script>

<div>
    <h3>gyro</h3>
    <button on:click={start}>{isStarted ? 'Stop' : 'Start'}</button>

    <h4>accelerometer</h4>
    {#each Object.entries(accelerometer) as [key, value]}
        <span>{key}: {value}</span>
    {/each}

    <h4>gyroscope</h4>
    {#each Object.entries(gyroscope) as [key, value]}
        <span>{key}: {value}</span>
    {/each}

    <h4>orientation</h4>
    {#each Object.entries(orientation) as [key, value]}
        <span>{key}: {value}</span>
    {/each}
</div>

<style lang="scss">
    div {
        display: flex;
        flex-direction: column;
    }

    span {
        font-family: monospace;
    }
</style>
