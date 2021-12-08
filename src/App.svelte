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
    };

    let gyroscope: { x: number; y: number; z: number };

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
        } else {
            window.removeEventListener('devicemotion', handleMotion);
        }

        isStarted = !isStarted;
    };
</script>

<div>
    <h3>gyro</h3>
    <button on:click={start}>{isStarted ? 'Stop' : 'Start'}</button>
    <span>{JSON.stringify(accelerometer)}</span>
    <span>{JSON.stringify(gyroscope)}</span>
</div>

<style lang="scss">
    div {
        display: flex;
        flex-direction: column;
    }
</style>
