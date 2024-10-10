<script>
    import { onMount } from 'svelte';
    import { goto } from '$app/navigation';
    import longVideo from '$lib/videos/wuxing.mp4';

    /**
     * @type {HTMLVideoElement}
     */
    let videoLong;

    /**
     * @type {HTMLElement}
     */
    let longVideoSection;

    // Throttle function to limit scroll-triggered updates
    /**
	 * @param {{ (): void; (arg0: any): any; }} fn
	 * @param {number} limit
	 */
    function throttle(fn, limit) {
        let lastCall = 0;
        return function (/** @type {any} */ ...args) {
            const now = new Date().getTime();
            if (now - lastCall >= limit) {
                lastCall = now;
                // @ts-ignore
                return fn(...args);
            }
        };
    }

    // Throttled scroll event handler for the video playback
    const throttledScroll = throttle(scrollLong, 50); // 100ms throttle time

    async function handleScroll() {
        const scrollPosition = window.scrollY + window.innerHeight;
        const pageHeight = document.documentElement.scrollHeight;

        if (scrollPosition >= pageHeight) {
            await goto('/', { replaceState: false, noScroll: false });  // Go to the next page
        }
    }

    function scrollLong() {
        const distance = window.scrollY - longVideoSection?.offsetTop;
        const total = longVideoSection?.clientHeight - window.innerHeight;

        let percentage = distance / total;
        percentage = Math.max(0, percentage);
        percentage = Math.min(percentage, 1);

        if (videoLong && videoLong.duration > 0) {
            videoLong.currentTime = videoLong.duration * percentage;
        }
    }

    onMount(() => {
        // Add the throttled scroll event listener for video playback
        window.addEventListener('scroll', throttledScroll);

        // Add the scroll event listener for page navigation
        window.addEventListener('scroll', handleScroll);

        // Cleanup event listeners when component is destroyed
        return () => {
            window.removeEventListener('scroll', throttledScroll);
            window.removeEventListener('scroll', handleScroll);
        };
    });
</script>

<main>
    <div class="additional-content">
        <!-- Your additional content goes here -->
        <h2>Page5 Go down</h2>
        <h2>you can't control the flow of time, but that can be harnessed</h2>

        <section bind:this={longVideoSection} class="vid long-vid">
            <div class="holder">
                <video bind:this={videoLong} id="videoLongPlayer" src={longVideo} muted playsinline></video>
            </div>
        </section>

        <p>Scroll down</p>
    </div>
</main>

<style>
    section.vid {
        height: 600vh;
        position: relative;
    }

    section.vid div.holder {
        position: sticky;
        top: 0;
    }

    section.vid video {
        width: 80%;
        height: 80%;
        object-fit: cover;
        padding-left: 100px;
    }
</style>

