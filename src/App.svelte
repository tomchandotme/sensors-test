<script lang="ts">
    let video: HTMLVideoElement;
    let errorMsg = '';

    const handleButtonClick = async () => {
        errorMsg = '';
        try {
            await video.requestPictureInPicture();
        } catch (error) {
            errorMsg = error;
        }
    };

    const requestCamera = async () => {
        errorMsg = '';
        try {
            const mediaStream = await navigator.mediaDevices.getUserMedia({
                video: true,
            });
            video.srcObject = mediaStream;
        } catch (error) {
            errorMsg = error;
        }
    };
</script>

<div>
    <h3>cam pip</h3>
    <video bind:this={video} width={128} height={128} autoplay />
    <span>{errorMsg}</span>
    <button on:click={handleButtonClick}>pip</button>
    <button on:click={requestCamera}>cam</button>
</div>

<style lang="scss">
    div {
        display: flex;
        flex-direction: column;
    }

    video {
        background-color: #888;
    }
</style>
