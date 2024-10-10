<script>
  import { onMount } from 'svelte';
  import { goto } from '$app/navigation';
  import appleVideo from '$lib/videos/applefallreverse.mp4'

  let autoScrollTriggered = false; 
  /**
	 * @type {HTMLVideoElement | null}
	 */
  let videoSnake; // Reference to the video element
  // let videoSrc = "src/lib/videos/applefallreverse.mp4"; // Add your video source here
  /**
	 * @type {HTMLElement | null}
	 */
  let videoSection;


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
    const throttledScroll = throttle(handleScroll, 300); // 100ms throttle time


  async function handleScroll() {
    // const section = document.querySelector('section.snake-vid'); // Get the video section
    const distance = window.scrollY - (videoSection?.offsetTop ?? 0);  // Distance from top of the section to the current scroll position
    const total = videoSection?.clientHeight ?? 0; // Total height of the section

    // Check if the scroll is within the bounds of the section
    if (distance >= 0 && distance <= total) {
      let percentage = (distance) / (total); // Calculate percentage from top
      percentage = Math.max(0, percentage); // Ensure percentage is between 0 and 1
      percentage = Math.min(percentage, 1);
      
      // Reverse percentage for scrolling up
      if (videoSnake && videoSnake.duration > 0) {
        videoSnake.currentTime = videoSnake.duration * (1 - percentage); // Reverse the playback
      }
    }

    // Logic to go to another page when the top is reached
    if (window.scrollY === 0) {
      await goto('page4', { replaceState: false, noScroll: false });
    }
  }

  onMount(() => {
    // videoSnake = document.getElementById('videoSnakePlayer'); // Get video element
    window.addEventListener('scroll', handleScroll);
    window.addEventListener('scroll', throttledScroll);

    return () => {
      window.removeEventListener('scroll', handleScroll);
      window.removeEventListener('scroll', throttledScroll);
    };
  });
</script>

<main>
  <div class="additional-content">
    <!-- Your additional content goes here -->
    <h2>Page 3 </h2>

  </div>

  <!-- Snake Video Section -->
  <section bind:this={videoSection} class="vid snake-vid">


    <div class="holder">
      <video bind:this={videoSnake} id="videoSnakePlayer" src={appleVideo} muted playsinline></video>
    </div>
  </section>
  <p>Scroll up to play the video</p>

  <div id="marker"></div>
</main>

<style>
  main {
    font-family: sans-serif;
  }

  section.vid {
   
    height: 200vh;
 
    display: flex;
    justify-content: center;
    align-items: center;
  }


  section.vid div.holder {
    width: 100%;
    position: sticky;
    top: 0; /* Sticks to the top when you scroll */
    z-index: 1; /* Ensure it appears above other content */
    justify-content: center;
  }

  section.vid video {
    width: 100%;
    max-width: 1400px;
    height: auto;
  }
</style>

