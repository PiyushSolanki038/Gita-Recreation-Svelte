<script>
  import { onMount } from "svelte";

  /* ===================== STATE ===================== */

  let viewMode = "grid";
  let selectedChapter = null;

  let showChapterModal = false;
  let modalChapter = null;
  let modalVerse = null;

  let showFontPanel = false;
  let modalFontSize = 18;

  let chapterData = null;
  let isLoadingText = false;
  let apiError = null;
  let modalText = "";

  const verseCounts = {
    1: 47, 2: 72, 3: 44, 4: 43, 5: 30, 6: 48,
    7: 31, 8: 29, 9: 35, 10: 43, 11: 56, 12: 21,
    13: 35, 14: 28, 15: 21, 16: 25, 17: 29, 18: 79
  };

  /* ===================== FUNCTIONS ===================== */

  function openChapter(i) {
    selectedChapter = i + 1;
    viewMode = "chapter";
  }

  function backToGrid() {
    viewMode = "grid";
    selectedChapter = null;
  }

  // Load chapter content when modal opens
  $: if (showChapterModal && modalChapter && chapterData === null) {
    loadChapter(modalChapter);
  }
async function loadChapter(ch) {
  isLoadingText = true;
  apiError = null;
  chapterData = null;

  try {
    const url = `https://sanskrit.ie/api/geeta.php?q=${ch}`;
    const proxyUrl = `https://api.allorigins.win/raw?url=${encodeURIComponent(url)}`;

    const res = await fetch(proxyUrl);
    if (!res.ok) throw new Error("API error");

    const json = await res.json();
    chapterData = json.data;

  } catch (err) {
    console.error(err);
    apiError = "Could not load verses.";
  } finally {
    isLoadingText = false;
  }
}

  // Extract text from API
  function getVerseText(v) {
    return v?.lyrics || "";
  }

  // Build modal text reactively
  $: {
    if (chapterData) {
      if (modalVerse) {
        modalText = getVerseText(chapterData[modalVerse - 1]);
      } else {
        modalText = chapterData.map(v => getVerseText(v)).join("<br/><br/>");
      }
    }
  }

  function decodeHTML(encoded) {
  const txt = document.createElement("textarea");
  txt.innerHTML = encoded;
  return txt.value;
}

  let showScrollTop = false;

  onMount(() => {
    window.addEventListener("scroll", () => {
      showScrollTop = window.scrollY > 200;
    });
  });
</script>

<!-- ====================== HERO SECTION ====================== -->

<section class="gita-hero">
  <div class="gita-bg"></div>
  <div class="gita-strip"></div>
  <h1 class="gita-title">BHAGAVAD GITA</h1>

  <div class="gita-book">
    <img src="/gita_open.png" alt="Open Book" />
  </div>
</section>

<!-- ====================== CHAPTERS SECTION ====================== -->

<section class="gita-chapters-section" id="chapters-top">
  <div class="chapters-inner">

    {#if viewMode === "grid"}
      <h2 class="chapters-title">
        GITA CHAPTERS
        <span class="chapters-underline"></span>
      </h2>
    {/if}

    {#if viewMode === "grid"}
      <!-- GRID VIEW -->
      <div class="chapters-grid">
        {#each Array(18) as _, i}
          <button type="button" class="chapter-card" on:click={() => openChapter(i)}>
            <div class="chapter-img-wrap">
              <img src="/gita_book.jpg" alt="chapter" />
              <div class="chapter-overlay">
                <span class="chapter-number">{i + 1}</span>
              </div>
            </div>
          </button>
        {/each}
      </div>
    {:else}
      <!-- CHAPTER VIEW -->
      <div id="chapter-panel">
        <button type="button" class="chapter-back" on:click={backToGrid}>
          &lt; Back
        </button>

        <h2 class="chapter-heading">CHAPTER {selectedChapter}</h2>

        <div class="chapter-verse-header">
          <span>Verse</span>
        </div>

        <div class="chapter-scroll-row">

          <!-- Whole chapter -->
          <div class="scroll-card">
            <img src="/sletter.png" class="scroll-bg" />
            <div class="scroll-label">Whole<br>Chapter</div>

            <button
              class="scroll-play"
              on:click={() => {
                modalChapter = selectedChapter;
                modalVerse = null;
                showChapterModal = true;
              }}
            >
              ▶
            </button>
          </div>

          <!-- All verses -->
          {#if selectedChapter && verseCounts[selectedChapter]}
            {#each Array(verseCounts[selectedChapter]) as _, i}
              <div class="scroll-card">
                <img src="/sletter.png" class="scroll-bg" />

                <div class="scroll-label">
                  {#if i === verseCounts[selectedChapter] - 1}
                    End<br>Chapter
                  {:else}
                    {i + 1}
                  {/if}
                </div>

                <button
                  class="scroll-play"
                  on:click={() => {
                    modalChapter = selectedChapter;

                    const isLast = i === verseCounts[selectedChapter] - 1;
                    modalVerse = isLast ? null : i + 1;

                    showChapterModal = true;
                  }}
                >
                  ▶
                </button>
              </div>
            {/each}
          {/if}

        </div>
      </div>
    {/if}

  </div>
</section>

<!-- ====================== MODAL ====================== -->

{#if showChapterModal}
  <div class="chapter-modal-backdrop" on:click={() => (showChapterModal = false)}>

    <button class="chapter-modal-close" on:click={() => (showChapterModal = false)}>
      ✕
    </button>

    <div class="chapter-modal" on:click|stopPropagation>

      <div class="chapter-modal-inner">

        <div class="chapter-modal-header">
          <h2 class="chapter-modal-title">
            {#if modalVerse}
              Chapter {modalChapter}, Verse {modalVerse}
            {:else}
              Chapter {modalChapter} – Whole Chapter
            {/if}
          </h2>

          {#if !showFontPanel}
            <button class="chapter-modal-projector" on:click={() => (showFontPanel = true)}>
              <img src="/projector_black.png" alt="" />
            </button>
          {:else}
            <button class="chapter-modal-projector" on:click={() => (showFontPanel = false)}>
              ✕
            </button>
          {/if}
        </div>

        {#if showFontPanel}
          <div class="font-panel">
            <span class="font-panel-label">Font Size:</span>
            <span class="font-panel-min">12px</span>
            <input type="range" min="12" max="65" bind:value={modalFontSize} />
            <span class="font-panel-max">65px</span>
          </div>
        {/if}

        <div class="chapter-modal-text" style={`font-size: ${modalFontSize}px;`}>
          {#if isLoadingText}
            Loading…
          {:else if apiError}
            {apiError}
          {:else if chapterData}
            {@html decodeHTML(modalText)}
          {/if}
        </div>

      </div>

      <!-- Audio Player -->
      <div class="chapter-modal-audio">
        {#if chapterData}
          <audio
            controls
            src={
              modalVerse != null
                ? `https://sanskrit.ie/${chapterData[modalVerse - 1]?.music}`
                : `https://sanskrit.ie/${chapterData[0]?.music}`
            }
          ></audio>
        {/if}
      </div>

    </div>
  </div>
{/if}

<!-- ====================== SCROLL TOP BUTTON ====================== -->

{#if showScrollTop}
  <button class="scroll-top-btn" on:click={() => window.scrollTo({ top: 0, behavior: "smooth" })}>
    <svg width="26" height="26" viewBox="0 0 24 24" fill="none" stroke="#ffffff" stroke-width="3">
      <polyline points="18 15 12 9 6 15"></polyline>
    </svg>
  </button>
{/if}

<style>

/* ------------------ HERO ------------------ */

.gita-hero {
  position: relative;
  width: 100%;
  height: 500px;
  background: #f3ead4;
}

.gita-bg {
  position: absolute;
  top: 100px;
  width: 100%;
  height: 400px;
  background-image: url('/gita_banner.png');
  background-size: cover;
  background-position: center;
  filter: brightness(75%);
}

.gita-strip {
  position: absolute;
  top: 20px;
  width: 100%;
  height: 150px;
  background-color: #ffffffa3;
  backdrop-filter: blur(3px);
}

.gita-title {
  position: absolute;
  top: -25px;
  width: 100%;
  font-family: 'edensor', 'Noto Sans Devanagari' !important;
  text-align: center;
  font-size: 100px;
  letter-spacing: 7px;
  color: #3B4634;
  font-weight: 300;
  text-shadow: 0px 4px 9px rgba(0,0,0,0.25);
}

.gita-book {
  position: absolute;
  bottom: -80px;
  width: 100%;
  display: flex;
  justify-content: center;
  z-index: 5;
}

.gita-book img {
  width: 260px;
}

/* ------------------ CHAPTERS ------------------ */

.gita-chapters-section {
  padding: 80px 0 200px;
  background-image: url('/pic1.jpg');
  background-position: 40% top;
  background-size: cover;
}

.chapters-inner {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 40px;
}

.chapters-title {
  text-align: center;
  font-family: "Cinzel", serif;
  font-size: 28px;
  letter-spacing: 3px;
  color: #b2192b;
  margin-bottom : 70px;
}

.chapters-underline {
  display: block;
  width: 350px;
  height: 2px;
  background: #b2192b;
  margin: 10px auto 0;
}

.chapters-grid {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  row-gap: 70px;
  column-gap: 80px;
  justify-items: center;
}

.chapter-card {
  width: 140px;
  height: 140px;
  border: none;
  background: transparent;
  cursor: pointer;
}

.chapter-img-wrap {
  width: 100%;
  height: 100%;
  position: relative;
  transform: rotate(45deg);
  border: 2px solid #b3a594;
  border-radius: 12px;
  overflow: hidden;
}

.chapter-img-wrap img {
  width: 130%;
  height: 130%;
  object-fit: cover;
  transform: rotate(-45deg);
  transform: rotate(-45deg) translateY(-25px); 
  transform: rotate(-45deg) translate(15px ,-20px);

}

.chapter-overlay {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%) rotate(-45deg);
  width: 220px;
  height: 32px;
  background: rgba(0,0,0,0.55);
  display: flex;
  align-items: center;
  justify-content: center;
}

.chapter-number {
  color: white;
  font-size: 26px;
}

/* ------------------ CHAPTER VIEW ------------------ */

.chapter-back {
  border: none;
  background: transparent;
  color: #b2192b;
  font-size: 16px;
  margin-bottom: 20px;
}

.chapter-heading {
  text-align: center;
  font-family: "Cinzel", serif;
  font-size: 32px;
  color: #b2192b;
  margin-bottom: 30px;
  text-decoration: underline;
}

.chapter-verse-header {
  border-top: 2px solid #b2192b;
  border-bottom: 2px solid #b2192b;
  padding: 18px 0;
  margin-bottom: 36px;
  font-size: 18px;
  color: #b2192b;
  text-align: left;
}

.chapter-verse-header span {
  margin-left: 20px;
  font-size: 25px;
}

.chapter-scroll-row {
  display: flex;
  flex-wrap: wrap;
  gap: 40px 60px;
  justify-content: center;
}

.scroll-card {
  position: relative;
  width: 180px;
  height: 260px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.scroll-bg {
  width: 500%;
  height: 100%;
  object-fit: contain;
}

.scroll-label {
  position: absolute;
  top: 36%;
  left: 50%;
  transform: translate(-50%, -50%);
  color: white;
  font-size: 26px;
  text-align: center;
}

.scroll-play {
  position: absolute;
  bottom: 18%;
  left: 50%;
  transform: translateX(-50%);
  width: 52px;
  height: 52px;
  border-radius: 50%;
  border: 3px solid white;
  background: rgba(0,0,0,0.6);
  color: white;
  cursor: pointer;
}

/* ------------------ MODAL ------------------ */

.chapter-modal-backdrop {
  position: fixed;
  inset: 0;
  background: rgba(0,0,0,0.5);
  backdrop-filter: blur(4px);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 999;
}

.chapter-modal {
  width: min(1100px, 95vw);
  max-height: 90vh;
  background: #f3c69a;
  border-radius: 18px;
  display: flex;
  flex-direction: column;
}

.chapter-modal-inner {
  padding: 32px 40px 24px;
  overflow-y: auto;
  flex: 1;
}

.chapter-modal-header {
  text-align: center;
  margin-bottom: 10px;
  position: relative;
}

.chapter-modal-title {
  font-family: "Cinzel", serif;
  font-size: 28px;
}

.chapter-modal-projector {
  position: absolute;
  right: 0;
  top: 0;
  background: transparent;
  border: none;
}

.font-panel {
  display: flex;
  gap: 8px;
  margin-bottom: 18px;
  padding: 4px;
}

.chapter-modal-text {
  font-size: 16px;
  line-height: 1.7;
}

.chapter-modal-audio {
  padding: 16px;
  background: transparent !important;
  border-top: none !important;
  box-shadow: none !important;

  position: sticky;
  bottom: 0;
  z-index: 10;
}

.chapter-modal-audio audio {
  width: 100%;
  height: 45px;

  background: transparent !important;
  border: none !important;
  box-shadow: none !important;
  outline: none !important;
}

audio::-webkit-media-controls-panel {
  background: transparent !important;
}



.chapter-modal-close {
  position: fixed;
  top: 16px;
  right: 24px;
  font-size: 26px;
  background: transparent;
  color: white;
  border: none;
  cursor: pointer;
}

/* ------------------ SCROLL TOP ------------------ */

.scroll-top-btn {
  position: fixed;
  bottom: 26px;
  right: 26px;
  width: 52px;
  height: 52px;
  border-radius: 12px;
  background: #3b372f;
  border: none;
  cursor: pointer;
  box-shadow: 0 4px 14px rgba(0,0,0,0.45);
  z-index: 2000;
}

.scroll-top-btn:hover { transform: scale(1.1); }
/* ==============================
   CHAPTERS SECTION – RESPONSIVE
============================== */

@media (max-width: 900px) {
  .gita-title {
    font-size: 70px;
  }
}

@media (max-width: 768px) {
  .gita-title {
    font-size: 48px;
  }

  .chapters-title {
    font-size: 22px;
  }

  .chapters-grid {
    grid-template-columns: repeat(2, 1fr);
    row-gap: 40px;
    column-gap: 40px;
  }

  .chapter-card {
    width: 110px;
    height: 110px;
  }

  .chapter-number {
    font-size: 20px;
  }

  /* Modal */
  .chapter-modal {
    width: 95vw;
  }

  .chapter-modal-title {
    font-size: 20px;
  }

  .chapter-modal-text {
    font-size: 14px !important;
  }

  .chapter-verse-header {
    width: 100%;
    margin-left: 0;
    text-align: center;
  }
}

@media (max-width: 480px) {
  .gita-title {
    font-size: 36px;
  }

  .chapters-grid {
    grid-template-columns: repeat(2, 1fr);
  }

  .chapter-card {
    width: 95px;
    height: 95px;
  }

  .chapter-modal-text {
    font-size: 13px !important;
  }
}

</style>
