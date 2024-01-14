<template>
  <div class="bg-[#0e192e] min-h-screen relative">
    <div class="flex flex-col items-center justify-center min-h-screen">
      <a href="https://liquidweb.i3f2.net/Jzv2R2" class="md:absolute md:top-0">
        <img alt="ad" src="/liquid.png" class="pt-4 md:hidden">
        <img alt="ad" class="md:block hidden" src="/ad-dedicated-hosting-affiliate-970x90-q3-2023.png"/>
      </a>
      <h1 class="text-3xl mt-5 md:mt-0 text-white font-bold">BinClock</h1>
      <p class="text-slate-300 pt-5">This is a simple binary clock.</p>
      <canvas id="canvas" height="500" width="700" class="pt-10 w-full" style="max-width: 40rem"></canvas>
      <div class="pt-10 flex flex-row">
        <a href="https://github.com/BenHerbst/BinClock" type="button" class="text-[#11192e] flex justify-center transition-all active:scale-90 bg-[#57c5ab] font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2 focus:outline-none">GitHub</a>
        <a href="https://github.com/BenHerbst/BinClockMobile" type="button" class="text-[#11192e] flex justify-center transition-all active:scale-90 bg-[#57c5ab] font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2 focus:outline-none">Mobile</a>
        <a href="https://github.com/AmirAli-AZ/BinaryClock" type="button" class="text-[#11192e] flex justify-center transition-all active:scale-90 bg-[#57c5ab] font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2 focus:outline-none">Desktop</a>
      </div>
      <a href="https://www.paypal.com/donate/?hosted_button_id=C5X9LBEM7XZ64" type="button" class="mt-3 text-gray-900 bg-[#F7BE38] active:scale-90 transition-all font-medium rounded-lg text-sm px-5 py-2.5 text-center inline-flex items-center dark:focus:ring-[#F7BE38]/50 me-2 mb-2">
        <svg class="w-4 h-4 me-2 -ms-1" aria-hidden="true" focusable="false" data-prefix="fab" data-icon="paypal" role="img" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 384 512"><path fill="currentColor" d="M111.4 295.9c-3.5 19.2-17.4 108.7-21.5 134-.3 1.8-1 2.5-3 2.5H12.3c-7.6 0-13.1-6.6-12.1-13.9L58.8 46.6c1.5-9.6 10.1-16.9 20-16.9 152.3 0 165.1-3.7 204 11.4 60.1 23.3 65.6 79.5 44 140.3-21.5 62.6-72.5 89.5-140.1 90.3-43.4 .7-69.5-7-75.3 24.2zM357.1 152c-1.8-1.3-2.5-1.8-3 1.3-2 11.4-5.1 22.5-8.8 33.6-39.9 113.8-150.5 103.9-204.5 103.9-6.1 0-10.1 3.3-10.9 9.4-22.6 140.4-27.1 169.7-27.1 169.7-1 7.1 3.5 12.9 10.6 12.9h63.5c8.6 0 15.7-6.3 17.4-14.9 .7-5.4-1.1 6.1 14.4-91.3 4.6-22 14.3-19.7 29.3-19.7 71 0 126.4-28.8 142.9-112.3 6.5-34.8 4.6-71.4-23.8-92.6z"></path></svg>
        Donate with PayPal
      </a>
    </div>
  </div>
</template>

<script lang="ts" setup>
// flex justify-center text-white bg-gray-800 transition-all active:scale-90 font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2
  
const ctx = ref<CanvasRenderingContext2D | null>(null)
const liquidLink = "https://liquidweb.i3f2.net/Jzv2R2"

useServerSeoMeta({
  title: "BinClock - Binary Clock for the 21th century",
  description: 'A simple and easy to use binary clock, with a UI of the 21th century. Never use any other clock again!',
  ogDescription: 'A simple and easy to use binary clock, with a UI of the 21th century. Never use any other clock again!',
  ogTitle: "B" +
      "inClock",
  ogImage: "https://clock.benherbst.net/img.png",
  twitterCard: 'summary_large_image',
  twitterSite: 'BinClock',
  twitterSiteId: 'for_devs79978',
  themeColor: '#40c7ab',
  applicationName: "BinClock",
  ogType: 'website',
  twitterImage: 'https://clock.benherbst.net/img.png'
})

const settings = reactive({
  primaryColor: '#40c7ab',
  lineWidth: 5,
  radius: 40,
})

onMounted(() => {
  const canvas = document.getElementById("canvas");
  if (canvas) ctx.value = (canvas as HTMLCanvasElement).getContext("2d")
  else throw new Error("No canvas")

  window.setInterval(applyTime, 1000)

  applyTime()
})

function applyTime() {
  const currentDate = new Date()

  if (ctx.value) ctx.value.clearRect(0, 0, 1000, 1000)

  const hours = currentDate.getHours();
  const mins = currentDate.getMinutes();
  const secs = currentDate.getSeconds();

  let position = 0;

  for (const time of [hours, mins, secs]) {
    for (let i = 0; i < 2; i++) {
      position++;
      let number;
      if (i % 2 === 0) {
        number = getFirstDigit(time)
      } else {
        number = getLastDigit(time)
      }
      applyBinary(number, position)
    }
  }
}


function getFirstDigit(input: number) {
  return getDigit(input, 0)
}

function getLastDigit(input: number) {
  return getDigit(input, 1)
}

function getDigit(input: number, pos: number) {
  return parseInt(input.toString().padStart(2, '0')[pos])
}

function applyBinary(input: number, x: number) {
  if (ctx.value) {
    const xPos = x * 100
    let name = x < 3 ? 'H' : x < 5 ? 'M' : 'S'

    ctx.value.strokeStyle = settings.primaryColor;
    ctx.value.fillStyle = settings.primaryColor;
    ctx.value.font = "48px arial";
    ctx.value.fillText(name, xPos - 18, 50);

    const binary = input.toString(2).padStart(4, '0')
    let index = 0;

    for(const number of binary) {
      ctx.value.beginPath();

      index++;
      ctx.value.arc(xPos, index * 100 + 20, settings.radius, 0, 2 * Math.PI, false);

      if (parseInt(number) == 1) {
        ctx.value.fill();
      }

      ctx.value.lineWidth = settings.lineWidth;
      ctx.value.stroke();

      ctx.value.closePath();
    }
  }
}
</script>
