<script>
  import Navbar from "$lib/components/Navbar.svelte";
  import Chapters from "$lib/components/Chapters.svelte";
  import Footer from "$lib/components/Footer.svelte";
</script>

<Navbar />
<Chapters />
<Footer />

<style>
  :global(body) {
    margin: 0;
    padding: 0;
    background: #ffffff;
    overflow-x: hidden;
    font-family: system-ui, sans-serif;
  }

  /* ==============================
   PAGE LAYOUT – RESPONSIVE
============================== */

@media (max-width: 768px) {
  .gita-hero {
    height: 380px;
  }

  .gita-title {
    font-size: 50px;
    letter-spacing: 4px;
  }

  .gita-book img {
    width: 180px;
  }

  .scroll-top-btn {
    width: 48px;
    height: 48px;
    bottom: 20px;
    right: 20px;
  }
}

@media (max-width: 480px) {
  .gita-hero {
    height: 320px;
  }

  .gita-title {
    font-size: 36px;
  }

  .gita-book img {
    width: 150px;
  }

  .scroll-top-btn {
    width: 44px;
    height: 44px;
  }
}

</style>
