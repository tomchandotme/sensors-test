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

<div class="overlay">
    <div
        class="cube"
        style="--rotate-x:{orientation.x};--rotate-y:{orientation.y};--rotate-z:{orientation.z};"
    >
        <div class="cube__face cube__face--front">front</div>
        <div class="cube__face cube__face--back">back</div>
        <div class="cube__face cube__face--right">right</div>
        <div class="cube__face cube__face--left">left</div>
        <div class="cube__face cube__face--top">top</div>
        <div class="cube__face cube__face--bottom">bottom</div>
    </div>
</div>

<style lang="scss">
    div {
        display: flex;
        flex-direction: column;
    }

    span {
        font-family: monospace;
    }

    div.overlay {
        z-index: -1;
        position: absolute;
        top: 0;
        left: 0;

        width: 100%;
        height: 100%;

        display: flex;
        align-items: center;
        justify-content: center;

        perspective: 50vmax;

        .cube {
            width: 20vmax;
            height: 20vmax;
            position: relative;
            transform-style: preserve-3d;
            transform: translateZ(-10vmax) rotateX(var(--rotate-x) deg)
                rotateY(var(--rotate-y) deg) rotateZ(var(--rotate-z) deg);
        }

        .cube__face {
            position: absolute;
            width: 20vmax;
            height: 20vmax;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .cube__face--front {
            background: hsla(0, 100%, 50%, 0.7);
        }
        .cube__face--right {
            background: hsla(60, 100%, 50%, 0.7);
        }
        .cube__face--back {
            background: hsla(120, 100%, 50%, 0.7);
        }
        .cube__face--left {
            background: hsla(180, 100%, 50%, 0.7);
        }
        .cube__face--top {
            background: hsla(240, 100%, 50%, 0.7);
        }
        .cube__face--bottom {
            background: hsla(300, 100%, 50%, 0.7);
        }

        .cube__face--front {
            transform: rotateY(0deg) translateZ(10vmax);
        }
        .cube__face--right {
            transform: rotateY(90deg) translateZ(10vmax);
        }
        .cube__face--back {
            transform: rotateY(180deg) translateZ(10vmax);
        }
        .cube__face--left {
            transform: rotateY(-90deg) translateZ(10vmax);
        }
        .cube__face--top {
            transform: rotateX(90deg) translateZ(10vmax);
        }
        .cube__face--bottom {
            transform: rotateX(-90deg) translateZ(10vmax);
        }
    }
</style>
