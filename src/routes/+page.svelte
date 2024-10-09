<script>
    import { onMount } from 'svelte';
    import { goto } from '$app/navigation';
   

    import videoSrc from '$lib/videos/heliogeo.mp4'; // Adjust the path to your video

    //horizontal text 
    let scrollPosition ;
    let textTranslateX = '-100vw';  // Start from off-screen (left)
    const maxScroll = 7000; // Adjust this value to control when the text finishes moving (corresponding to the total scroll distance)



    /**
	 * @type {HTMLVideoElement}
	 */
    let video;

    /**
	 * @type {HTMLElement}
	 */
    let videoSection;

    /**
	 * @type {HTMLDivElement}
	 */
    let stickyElement;

    let isStuck = false;  // Sticky state for the image text
  
    async function handleScroll() {
      const scrollPosition = window.scrollY + window.innerHeight;
      const pageHeight = document.documentElement.scrollHeight;
  
      if (scrollPosition >= pageHeight) {
       await goto('/page2',{replaceState:false, noScroll:false});

      }


    }

    function scrollHelioGeo() {
        const section = document.querySelector('section.vid');
        const distance = window.scrollY - videoSection.offsetTop;
        const total = videoSection?.clientHeight - window.innerHeight;

        let percentage = distance / total;
        percentage = Math.max(0, percentage);
        percentage = Math.min(percentage, 1);

        if (video && video.duration > 0) {
            video.currentTime = video.duration * percentage;
        }
    }
  
    // Handle sticky state for image container text
    function handleStickyScroll() {
        // const stickyElement = document.querySelector('.sticky-text');
        const rect = stickyElement?.getBoundingClientRect();
        isStuck = rect.top <= window.innerHeight / 2 && rect.bottom >= window.innerHeight / 2;
    }
    
  // Function to handle scrolling and update the text position
  function TextHorizonScroll() {
    const scrollTextElement = document.querySelector('.scroll-text');
    if (!scrollTextElement) {
      console.error('Scroll text element not found');
      return;
    }

    scrollPosition = window.scrollY;

    // Calculate how far along the text should be moved horizontally
    const scrollProgress = Math.min(scrollPosition / maxScroll, 1); // Ensure it's between 0 and 1

    // Move the text from -100vw (off-screen left) to 0vw (centered on-screen horizontally)
    textTranslateX = `${(scrollProgress * 200) - 100}vw`;
  }

  function onScroll() {
    handleScroll();
    scrollHelioGeo();
    handleStickyScroll();
    TextHorizonScroll();
  }


    onMount(() => {
     

      window.addEventListener('scroll', onScroll);
      // window.addEventListener('scroll', scrollHelioGeo);
      // window.addEventListener('scroll', handleStickyScroll);
      // window.addEventListener('scroll', TextHorizonScroll);
      // video = document.getElementById('videoPlayer');
  
      return () => {
        window.removeEventListener('scroll', onScroll);
        // window.removeEventListener('scroll', scrollHelioGeo);
        // window.removeEventListener('scroll', handleStickyScroll);
        // window.removeEventListener('scroll', TextHorizonScroll);
      };
    });

  // Reactive update of the scroll text position
  $: if (typeof window !== 'undefined') {
     /**
     * @type {HTMLElement | null}
     */
    const scrollTextElement = document.querySelector('.scroll-text');
    if (scrollTextElement) {
      scrollTextElement.style.transform = `translateX(${textTranslateX})`;
    }
  }

  </script>
  
  <main>
    <div class="top">
      <h1>1</h1>
      <p>Scroll down for more...</p>
    </div>
    

    <div class="sticky-container">
      <h1>1</h1>
      <p>Scroll down for more...</p>
    </div>

    <div class="image-container">
              <!-- Sticky text on top of image -->
             
              <div class ="start">
                <h2>There is </h2>
                <p>a progression in history...</p>
                </div>

              <div class ="empty">
                <h2>There is </h2>
                <p>a progression in history...</p>
                </div>
           
              <div bind:this={stickyElement} class="sticky-text" class:isStuck={isStuck}>
                <h2>There is </h2>
                <p>a progression in history...</p>
            </div>
        
      <!-- svelte-ignore a11y-img-redundant-alt -->
      <img src="src/lib/images/solar.png" alt="Description of image" />
    </div>







    <!-- Video Section -->
    <section  bind:this={videoSection} class="vid" >
          <!-- The moving text -->
    <div class="scroll-text" style="transform: translateX({textTranslateX})">
      <h1>Or it might be going in circles...</h1>
  </div>
      <div class="holder">
          <video bind:this={video} id="videoPlayer" src={videoSrc} muted playsinline></video>
          
      </div>

  </section>


  <div class="additional-content">
    <!-- Your additional content goes here -->
    <h2>Go down</h2>
    <p>place holder</p>

    <span style="display:block; height:70vh; background-color:black;"></span>
    <p>place holder</p>
</div>

  </main>
  

  

  <style>
    /* Center the image horizontally and vertically */
    .image-container {
      position: relative;
      top: 0;
      left: 0;
      width: 100%;
    
      z-index: 0; /* Make sure image is behind the sticky content */
      display: flex;
      justify-content: center; /* Center the image horizontally */
      background-color: black; /* Ensure the background is black */
    
    }
  
    img {
      top:0;
      left:0;
      max-width: 80%;

      height: auto; /* Maintain aspect ratio */
      object-fit: contain; /* Make sure the image fits well */
    }

    .sticky-container {
      position: -webkit-sticky; /* For Safari */
    position: sticky;
    top: 0; /* Stick to the top */
    padding: 10px 0;
    color: white; /* Make text white */
    z-index: 100; /* Ensure it stays above other content */
    
    background: transparent; /* Transparent background */
  }

  .start {
        position: sticky;
        top:50%;
        left: 50%;
        height: 30%;

        transform: translate(-50%, -50%);
        color: white;
        text-align: center;
        z-index: 10;
        background: rgba(0, 0, 0, 0.5);
        padding: 20px;
        border-radius: 10px;
        /* transition: all 0.3s; */
    }

  .empty {
        position: fixed;
        top:50%;
        left: 50%;
        height: 30%;

        transform: translate(-50%, -50%);
        color: white;
        text-align: center;
        z-index: 10;
        background: rgba(0, 0, 0, 0.5);
        padding: 20px;
        border-radius: 10px;
        /* transition: all 0.3s; */
    }
      /* Sticky text on top of image */
      .sticky-text {
        position: sticky;
        top:50%;
        left: 50%;
        height: 30%;

        transform: translate(-50%, -50%);
        color: white;
        text-align: center;
        z-index: 10;
        background: rgba(0, 0, 0, 0.5);
        padding: 20px;
        border-radius: 10px;
        transition: all 0.3s;
    }

    /* Change the style when sticky text is stuck */
    .sticky-text.isStuck {
        background: mintcream;
        color: black;
        box-shadow: 0px 0px 1px 0px rgba(0, 0, 0, 0.2),
                    0px 0px 6px 0px rgba(0, 0, 0, 0.3);
    }

    

  .top {
  
    top: 0; /* Stick to the top */
    padding: 10px 0;
    color: black; /* Make text white */
    z-index: 100; /* Ensure it stays above other content */
    
    background: transparent; /* Transparent background */
  }



 
/* video section */
  section.vid {
        height: 600vh;
        position: relative;
     
    }

    section.vid div.holder {
        position: sticky;
        top: 0;
    }

    section.vid video {
        width: 100%;
        height: 100%;
        object-fit: cover;
    }
    section.vid div.scroll-text {
    position: fixed;
    top: 50%; /* Keep the text vertically centered */
    left: 0;  /* Start from the left of the screen */
    width: 30%; /* Ensure the text container spans the width of the screen */
    display: flex;
  
    color: white;
    z-index: 100;
    transform: translateX(-100vw); /* Start horizontally off-screen */
    transition: transform 0.3s ease; /* Smooth horizontal transition */
}

  </style>