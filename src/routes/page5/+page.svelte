<script>
    import { onMount } from 'svelte';
    import { goto } from '$app/navigation';
  
  async  function handleScroll() {
      const scrollPosition = window.scrollY + window.innerHeight;
      const pageHeight = document.documentElement.scrollHeight;
  
      if (scrollPosition >= pageHeight) {
      await  goto('/', {replaceState:false, noScroll:true});  // Go to the next page
      }
    }
  
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
    <h2>Page5 Go down!</h2>
    <p>place holder</p>

    <span style="display:block; height:300vh; background-color:black;"></span>
    <p>place holder</p>
</div>

  </main>