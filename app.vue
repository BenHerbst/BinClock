<template>
  <div class="bg-[#0e192e] h-screen">
    <div class="flex flex-col items-center justify-center h-screen">
      <h1 class="text-3xl text-white font-bold">BinClock</h1>
      <p class="text-slate-300 pt-5">This is a simple binary clock.</p>
      <canvas id="canvas" height="500" width="700" class="pt-10 w-full" style="max-width: 40rem"></canvas>
      <div class="pt-10 flex flex-col md:flex-row">
        <a href="https://github.com/BenHerbst/BinClock" type="button" class="text-[#11192e] transition-all active:scale-90 bg-[#57c5ab] font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2 focus:outline-none">GitHub by Ben Herbst</a>
        <a href="https://github.com/AmirAli-AZ/BinaryClock" type="button" class="text-white bg-gray-800 transition-all active:scale-90 font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2">Desktop by Amir Ali</a>
      </div>
    </div>
  </div>
</template>

<script lang="ts" setup>
const ctx: Ref<CanvasRenderingContext2D | null> = ref(null)

useSeoMeta({
  title: "BinClock - Binary Clock for the 21th century",
  description: 'A simple and easy to use binary clock, with a UI of the 21th century. Never use any other clock again!',
  ogDescription: 'A simple and easy to use binary clock, with a UI of the 21th century. Never use any other clock again!',
  ogTitle: "BinClock",
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
  lineWidth: 3,
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
      } else {
        ctx.value.lineWidth = settings.lineWidth;
        ctx.value.stroke();
      }

      ctx.value.closePath();
    }
  }
}
</script>
