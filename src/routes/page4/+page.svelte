<script>
    import { onMount } from 'svelte';
    import { goto } from '$app/navigation';
    // import longVideo from '$lib/videos/long.mp4'

      /**
	 * @type {HTMLVideoElement}
	 */
  let videoLong;
    /**
	 * @type {HTMLElement}
	 */
     let longVideoSection;
  
  async  function handleScroll() {
      const scrollPosition = window.scrollY + window.innerHeight;
      const pageHeight = document.documentElement.scrollHeight;
  
      if (scrollPosition >= pageHeight) {
      await  goto('page5', {replaceState:false, noScroll:false});  // Go to the next page
      }
    }

    function scrollLong() {
    // const section = document.querySelector('section.snake-vid');
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
    window.addEventListener('scroll', scrollLong);

    return () => {
      window.removeEventListener('scroll', scrollLong);

    };
  });
  
    onMount(() => {
      window.addEventListener('scroll', handleScroll);
  
      return () => {
        window.removeEventListener('scroll', handleScroll);
      };
    });


  </script>

<main>
<div class="additional-content">
    <!-- Your additional content goes here -->
    <h2>Page4Go down</h2>
    <h2>you can experience it at your own pace, in opposite directions, at your will</h2>

    <section bind:this={longVideoSection} class="vid long-vid">
        <!-- The horizontal moving text -->

    
        <div class="holder">
          <video  bind:this={videoLong} id="videoLongPlayer" src="https://res.cloudinary.com/dufnjfidt/video/upload/v1729714563/long_hiwa34.mp4" muted playsinline></video>
        </div>
      </section>

    <p>scroll down</p>

   
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