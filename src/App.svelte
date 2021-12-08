<script lang="ts">
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

        accelerometer.i = e.interval;
    };
    const handleOrientation = (e: DeviceOrientationEvent) => {
        gyroscope.x = e.beta;
        gyroscope.y = e.gamma;
        gyroscope.z = e.alpha;
    };

    const start = () => {
        if (
            DeviceMotionEvent &&
            typeof DeviceMotionEvent.requestPermission === 'function'
        ) {
            DeviceMotionEvent.requestPermission();
        }

        window.addEventListener('devicemotion', handleMotion);
        window.addEventListener('deviceorientation', handleOrientation);
    };
</script>

<div>
    <h3>gyro</h3>
    <button on:click={start}>Start</button>
    <span>{JSON.stringify(accelerometer)}</span>
    <span>{JSON.stringify(gyroscope)}</span>
</div>

<style lang="scss">
    div {
        display: flex;
        flex-direction: column;
    }
</style>
