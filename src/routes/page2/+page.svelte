<script>
  import { onMount } from 'svelte';
  import { goto } from '$app/navigation';
  import videoSrc from '$lib/videos/snake.mp4'; // Path to your snake video
  import applefallSrc from '$lib/videos/applefall.mp4'; // Path to your applefall video
  import newtonSrc from '$lib/videos/newton.mp4'; // Path to your newton video

  // Horizontal text scroll variables
  let scrollPosition;
  let textTranslateX = '100vw';  // Start from off-screen (left)
  const maxScroll = 4000; // Adjust this value to control when the text finishes moving

  // Vertical text scroll variables for applefall and newton sections
  let textTranslateY = '-100vh';  // Start from off-screen (top)
  let newtonTranslateY = '-100vh';  // Separate variable for Newton section

  /**
	 * @type {HTMLVideoElement}
	 */
  let videoSnake;
  /**
	 * @type {HTMLVideoElement}
	 */
  let videoApplefall;
  /**
	 * @type {HTMLVideoElement}
	 */
  let videoNewton;

  /**
	 * @type {HTMLElement}
	 */
  let snakeVideoSection;

  /**
	 * @type {HTMLElement}
	 */
  let appleVideoSection;

  /**
	 * @type {HTMLElement}
	 */
   let newtonVideoSection;

  async function handleScroll() {
    const scrollPosition = window.scrollY + window.innerHeight;
    const pageHeight = document.documentElement.scrollHeight;

    if (scrollPosition >= pageHeight) {
      await goto('/page3#marker', { replaceState: false, noScroll: false });
  //going to bottom
  // setTimeout(() => {
  //         // After navigation, scroll to the bottom of the next page
  //         window.scrollTo({
  //           top: document.body.scrollHeight,
  //           behavior: 'auto'  // Instant scroll to the bottom of the new page
  //         });
  //       }, 100);
  //   }
    }
  }

  function scrollSnake() {
    // const section = document.querySelector('section.snake-vid');
    const distance = window.scrollY - snakeVideoSection?.offsetTop;
    const total = snakeVideoSection?.clientHeight - window.innerHeight;

    let percentage = distance / total;
    percentage = Math.max(0, percentage);
    percentage = Math.min(percentage, 1);

    if (videoSnake && videoSnake.duration > 0) {
      videoSnake.currentTime = videoSnake.duration * percentage;
    }
  }

  function scrollApplefall() {
    // const section = document.querySelector('section.applefall-vid');
    const distance = window.scrollY - appleVideoSection.offsetTop;
    const total = appleVideoSection?.clientHeight - window.innerHeight;

    if (distance >= 0 && distance <= total) {
      let percentage = distance / total;
      percentage = Math.max(0, percentage);
      percentage = Math.min(percentage, 1);

      if (videoApplefall && videoApplefall.duration > 0) {
        videoApplefall.currentTime = videoApplefall.duration * percentage;
      }

      // Vertical text scroll logic for applefall
      const scrollProgress = percentage;
      let translateValueY = scrollProgress * 100;
      textTranslateY = `${translateValueY}vh`;
    } else {
      textTranslateY = '-100vh'; // Keeps text hidden above the viewport when not in section
    }
  }

  function scrollNewton() {
    // const section = document.querySelector('section.newton-vid');
    const distance = window.scrollY - newtonVideoSection.offsetTop;
    const total = newtonVideoSection?.clientHeight - window.innerHeight;

    if (distance >= 0 && distance <= total) {
      let percentage = distance / total;
      percentage = Math.max(0, percentage);
      percentage = Math.min(percentage, 1);

      if (videoNewton && videoNewton.duration > 0) {
        videoNewton.currentTime = videoNewton.duration * percentage;
      }

      // Vertical text scroll logic for Newton
      const scrollProgress = percentage;
      let translateValueY = 100-scrollProgress * 100;
      newtonTranslateY = `${translateValueY}vh`;
    } else {
      newtonTranslateY = '-100vh'; // Keeps text hidden above the viewport when not in section
    }
  }

  function TextHorizonScroll() {
    const scrollTextElement = document.querySelector('.scroll-text');
    if (!scrollTextElement) {
      console.error('Scroll text element not found');
      return;
    }

    scrollPosition = window.scrollY;
    const scrollProgress = scrollPosition / window.innerHeight;
    let translateValue = 100 - scrollProgress * 100;
    textTranslateX = `${translateValue}vw`;

    if (translateValue <= -100) {
      translateValue += 300;
      textTranslateX = `${translateValue}vw`;
    }
  }

  function onScroll() {
    handleScroll();
      scrollSnake();
      scrollApplefall();
      scrollNewton();
      TextHorizonScroll();
  }

  onMount(() => {
    window.addEventListener('scroll', onScroll);
    // window.addEventListener('scroll', scrollSnake);
    // window.addEventListener('scroll', scrollApplefall);
    // window.addEventListener('scroll', scrollNewton);
    // window.addEventListener('scroll', TextHorizonScroll);

    // videoSnake = document.getElementById('videoSnakePlayer');
    // videoApplefall = document.getElementById('videoApplefallPlayer');
    // videoNewton = document.getElementById('videoNewtonPlayer');

    return () => {
      window.removeEventListener('scroll', onScroll);
      // window.removeEventListener('scroll', scrollSnake);
      // window.removeEventListener('scroll', scrollApplefall);
      // window.removeEventListener('scroll', scrollNewton);
      // window.removeEventListener('scroll', TextHorizonScroll);
    };
  });

  $: if (typeof window !== 'undefined') {
    /**
     * @type {HTMLElement | null}
     */
    const scrollTextElement = document.querySelector('.scroll-text');
    if (scrollTextElement) {
      scrollTextElement.style.transform = `translateX(${textTranslateX})`;
    }

    /**
     * @type {HTMLElement | null}
     */
    const verticalTextElement = document.querySelector('.vertical-scroll-text');
    if (verticalTextElement) {
      verticalTextElement.style.transform = `translateY(${textTranslateY})`;
    }

    /**
     * @type {HTMLElement | null}
     */
    const newtonTextElement = document.querySelector('.newton-scroll-text');
    if (newtonTextElement) {
      newtonTextElement.style.transform = `translateY(${newtonTranslateY})`;
    }
  }
</script>

<main>

  <h1>Page 2</h1>
  <p>Scroll down for more</p>

  <!-- Snake Video Section -->
  <section bind:this={snakeVideoSection} class="vid snake-vid">
    <!-- The horizontal moving text -->
    <div class="scroll-text" style="left: {textTranslateX}vw">
      <h1>Going around... back and forth</h1>
    </div>

    <div class="holder">
      <video  bind:this={videoSnake} id="videoSnakePlayer" src={videoSrc} muted playsinline></video>
    </div>
  </section>

  <!-- Applefall Video Section -->
  <section bind:this={appleVideoSection} class="vid applefall-vid">
    <!-- The vertical moving text -->
    <div class="vertical-scroll-text">
      <h1>A false case could prove a correct theory to be wrong,</h1>
    </div>

    <div class="holder">
      <video bind:this={videoApplefall} id="videoApplefallPlayer" src={applefallSrc} muted playsinline></video>
    </div>
  </section>

  <!-- Newton Video Section -->
  <section bind:this={newtonVideoSection} class="vid newton-vid">
    <!-- The vertical moving text for Newton -->
    <div class="newton-scroll-text">
      <h1>But one true anomaly can't bring the dead theory back alive</h1>
    </div>

    <div class="holder">
      <video bind:this={videoNewton} id="videoNewtonPlayer" src={newtonSrc} muted playsinline></video>
    </div>
  </section>

  <div style="height: 600px;"></div>
  <div style="height: 600px;"></div>
  <p>Scroll until you reach the bottom...</p>
  <p>More text to fill the page...</p>
</main>

<style>
  /* Video section */
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

  /* Horizontal text scroll */
  section.vid div.scroll-text {
    position: fixed;
    top: 50%;
    left: 0;
    width: 30%;
    display: flex;
    color: white;
    z-index: 100;
    transform: translateX(-100vw);
    transition: transform 0.3s ease;
  }

  /* Vertical text scroll for Applefall */
  section.vid div.vertical-scroll-text {
    position: fixed;
    left: 10%;
    top: 0;
    transform: translateY(-100vh);
    transition: transform 0.3s ease;
    color: white;
    z-index: 100;
    font-size: 1.5rem;
    white-space: nowrap;
  }

  /* Vertical text scroll for Newton */
  section.vid div.newton-scroll-text {
    position: fixed;
    left: 15%;
    top: 0;
    transform: translateY(-100vh);
    /* transition: transform 0.3s ease; */
    color:black;
    z-index: 100;
    font-size: 1.5rem;
    white-space: nowrap;
  }
</style>

