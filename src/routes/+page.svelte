<script>
  import { onMount, tick } from "svelte";
  import { assets } from '$app/paths';


  let canvas;
  let ctx;
  let points = [];
  const maxPoints = 100;
  
  class Point {
    constructor(x, y) {
      this.x = x;
      this.y = y;
      this.vx = Math.random() * 2 - 1;
      this.vy = Math.random() * 2 - 1;
    }
  
    update() {
      this.x += this.vx;
      this.y += this.vy;
  
      if (this.x < 0 || this.x > canvas.width) this.vx *= -1;
      if (this.y < 0 || this.y > canvas.height) this.vy *= -1;
    }
  
    draw() {
      ctx.beginPath();
      ctx.arc(this.x, this.y, 2, 0, Math.PI * 2);
      ctx.fillStyle = "#ffffff";
      ctx.fill();
    }
  }
  
  function connectPoints() {
    for (let i = 0; i < points.length; i++) {
      for (let j = i + 1; j < points.length; j++) {
        const distance = Math.hypot(points[i].x - points[j].x, points[i].y - points[j].y);
        if (distance < 100) {
          ctx.beginPath();
          ctx.moveTo(points[i].x, points[i].y);
          ctx.lineTo(points[j].x, points[j].y);
          ctx.strokeStyle = `rgba(0, 170, 255, ${1 - distance / 100})`;
          ctx.stroke();
        }
      }
    }
  }
  
  function initPoints() {
    points = [];
    for (let i = 0; i < maxPoints; i++) {
      const x = Math.random() * canvas.width;
      const y = Math.random() * canvas.height;
      points.push(new Point(x, y));
    }
  }
  
  function animate() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
  
    points.forEach((point) => {
      point.update();
      point.draw();
    });
  
    connectPoints();
  
    requestAnimationFrame(animate);
  }
  
  let text = "Open source solutions";
  let displayText = "";
  let i = 0;

  const delay = 100;  // Tiempo en milisegundos entre cada letra

  const type = async () => {
    while (i < text.length) {
      await new Promise(resolve => setTimeout(resolve, delay));  // Espera antes de escribir la siguiente letra
      displayText += text[i++];
    }
  };

  $: type();  // Inicia la escritura cuando el componente se monta

  onMount(() => {
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;  // Altura del canvas a 30% de la altura de la ventana
    ctx = canvas.getContext("2d");
  
    initPoints();
    animate();
  
    const resizeHandler = () => {
      canvas.width = window.innerWidth;
      canvas.height = window.innerHeight; // Reajustamos el canvas cuando se redimensiona
      initPoints();
    };
  
    window.addEventListener("resize", resizeHandler);
  
    return () => {
      window.removeEventListener("resize", resizeHandler);
    };
  });
</script>

<style>
  canvas {
    position: absolute;  /* El canvas será absoluto para quedar debajo de otros elementos */
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;  /* El canvas ocupa el 30% de la altura de la ventana */
    z-index: -1; /* El canvas se sitúa debajo del contenido */
  }
</style>

<canvas bind:this={canvas} class="bg-gradient-to-r from-stone-900 to-slate-200"></canvas>

<!-- Contenedor de texto con posición relativa, que se coloca sobre el canvas -->
<div class="relative z-10 flex flex-col items-center justify-center pt-16 mt-64 mb-64 pb-6">
  <div class="flex items-center justify-center">
    <div class="rounded-xl bg-gradient-to-b from-green-600 to-slate-700 pr-5 pl-5 pt-1 pb-1 font-bold text-6xl text-white">b</div>
    <h1 class="text-6xl font-bold text-white drop-shadow-lg">itbaso</h1>
  </div>    
  
  <div class="mt-2 flex items-center">
    <div class=" pr-2">
      <h2 class="pl-20">{displayText}</h2>
    </div>
    <div>
      <a href="https://github.com/bitbaso" target="_blank" title="github bitbaso">
          <img src={`${assets}/github-mark-white.png`} width="20px" alt="bitbaso github" />
      </a>    
  </div>
  <div class="ml-4">
    <a href="mailto:bitbaso@gmail.com" target="_blank" title="bitbaso email" class="text-white">
      <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="size-6">
        <path stroke-linecap="round" stroke-linejoin="round" d="M21.75 6.75v10.5a2.25 2.25 0 0 1-2.25 2.25h-15a2.25 2.25 0 0 1-2.25-2.25V6.75m19.5 0A2.25 2.25 0 0 0 19.5 4.5h-15a2.25 2.25 0 0 0-2.25 2.25m19.5 0v.243a2.25 2.25 0 0 1-1.07 1.916l-7.5 4.615a2.25 2.25 0 0 1-2.36 0L3.32 8.91a2.25 2.25 0 0 1-1.07-1.916V6.75" />
      </svg>      
    </a>
  </div>
  </div>
</div>


<div class="relative bg-slate-600">
  <div class="bg-slate-500 pt-1 pb-2 text-white ">  
    <div class="p-4">
      <h3 class="font-bold">Section 1</h3>
      <p>This is the first section. Now there is no parallax effect, but the content is styled with a nice design!</p>
    </div>    
</div>
<div class=" bg-slate-600 mt-2 mb-2 ">
  <div class="bg-slate-500 pt-1 pb-2 text-white ">  
    <div class="p-4">
      <h3 class="font-bold">Section 1</h3>
      <p>This is the first section. Now there is no parallax effect, but the content is styled with a nice design!</p>
    </div>    
  </div>
</div>
  <div class=" bg-slate-600 mt-2 mb-2 ">
    <div class="bg-slate-500 pt-1 pb-2 text-white ">  
      <div class="p-4">
        <h3 class="font-bold">Section 1</h3>
        <p>This is the first section. Now there is no parallax effect, but the content is styled with a nice design!</p>
      </div>    
  </div>
</div>
  <div class=" bg-slate-600 mt-2 mb-2 ">
    <div class="bg-slate-500 pt-1 pb-2 text-white ">  
      <div class="p-4">
        <h3 class="font-bold">Section 1</h3>
        <p>This is the first section. Now there is no parallax effect, but the content is styled with a nice design!</p>
      </div>    
  </div>
</div>
</div>
