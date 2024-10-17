<script>
  import { onMount } from "svelte";
  import getHeroBanner from "../utils/shopify";

  // Reactive store to hold fetched data
  let data = [];
  let bannerImages = [];
  let heroImages;
  let currentIndex = 0;

  // Fetch the data on component mount
  onMount(async () => {
    try {
      const result = await getHeroBanner();
      if (result && Array.isArray(result)) {
        data = result;
        bannerImages = data
          .map((item, index) => {
            return `<img class="hero-images absolute w-screen" style="filter: brightness(0.6);" src="${item.image.url}" alt="${item.alt ?? "Generic Image"}" />`;
          })
          .join("");
      } else {
        console.error("Invalid data format");
      }
    } catch (error) {
      console.error("Error fetching data:", error);
    }
    const interval = setInterval(() => {
      showImage();
    }, 9000); // 6000 ms = 6 seconds
    return () => {
      clearInterval(interval);
    };
  });
  function showImage() {
    const imageElements = document.querySelectorAll(".hero-images");
    imageElements.forEach((img, index) => {
      img.style.display = index === currentIndex ? "block" : "none";
    });
    currentIndex = (currentIndex + 1) % data.length;
  }
</script>

<main class="hero-banner-container relative">
  <div
    class="hero-banner overflow-hidden w-screen bg-black/55 xl:h-[70vh] lg:h-[45vh] whitespace-nowrap relative"
    contenteditable="true"
    bind:innerHTML={bannerImages}
  >
    {bannerImages}
  </div>
  <div class=" absolute top-0 w-full h-full flex flex-col justify-center items-center gap-4">
    <h1 class="anton-regular text-white text-4xl">The Ikarus Experience is Now</h1>
    <p class="text-white w-1/2 text-center">Lorem ipsum dolor sit amet consectetur adipisicing elit. Ab minus maxime corporis? Numquam, molestias ratione. Quidem reiciendis adipisci dignissimos voluptatem.</p>
    <a class="button text-lg tracking-widest" href="#catalog">
        See More
    </a>
  </div>
</main>
