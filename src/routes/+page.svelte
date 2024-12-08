<script>
  import { onMount } from "svelte";
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
  
  let paralaxy = 0;

  // Detecta el scroll de la página
  onMount(() => {
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight * 0.3;  // El canvas ocupa solo el 30% de la altura
    ctx = canvas.getContext("2d");
  
    initPoints();
    animate();
  
    const resizeHandler = () => {
      canvas.width = window.innerWidth;
      canvas.height = window.innerHeight * 0.3;
      initPoints();
    };
  
    window.addEventListener("resize", resizeHandler);
  
    // Actualiza el paralaje en cada scroll
    window.addEventListener('scroll', () => {
      paralaxy = window.scrollY;
    });
  
    return () => {
      window.removeEventListener("resize", resizeHandler);
      window.removeEventListener('scroll', () => {});
    };
  });
</script>

<style>
  canvas {
    display: block;
    width: 100%;
    height: 100%;
    position: fixed;  /* El canvas permanece fijo en la parte superior */
    top: 0;
    left: 0;
    z-index: -1; /* Asegura que el canvas quede debajo de otros elementos */
  }

  :global(body, html) {
    margin: 0;
    padding: 0;
    height: 100%;
    overflow: auto; /* Permite el scroll */
  }

  .content-container {
    position: relative;
    min-height: 200vh; /* Esto garantiza suficiente contenido para scroll */
    padding-top: 30vh; /* Esto asegura que el contenido comienza debajo del canvas */
  }

  .parallax-item {
    position: relative;
    top: 0;
    transition: transform 0.1s ease-out;
    transform: translateY(calc(var(--paralaxy) * 0.5px)); /* Movimiento del parallax */
    margin: 20px 0;
    padding: 20px;
    background-color: #f0f0f0;
    border-radius: 10px;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  }

  .section-title {
    text-align: center;
    font-size: 2rem;
    font-weight: bold;
    margin-top: 50px;
  }

  /* Estilos del encabezado */
  .absolute {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    text-align: center;
    z-index: 10;
  }

  .absolute .flex {
    display: flex;
    justify-content: center;
    align-items: center;
  }
</style>

<canvas bind:this={canvas} class="bg-gradient-to-r from-stone-900 to-slate-200"></canvas>

<div class="absolute">
  <div class="flex items-center">
    <div class="rounded-xl bg-gradient-to-b from-green-600 to-slate-700 pr-5 pl-5 pt-1 pb-1 font-bold text-6xl text-white">b</div>
    <h1 class="text-6xl font-bold text-white drop-shadow-lg">itbaso</h1>
  </div>    
  <h2 class="mt-4 pl-20">Open source solutions</h2>
</div>

<!-- Contenedor con varios divs con parallax -->
<div class="content-container">
  <div class="parallax-item" style="--paralaxy: {paralaxy}">
    <h2 class="section-title">Section 1</h2>
    <p>This is the first section with parallax effect. Scroll down to see the effect!</p>
  </div>
  <div class="parallax-item" style="--paralaxy: {paralaxy}">
    <h2 class="section-title">Section 2</h2>
    <p>This is the second section with more content. The parallax effect makes it feel like the content is floating!</p>
  </div>
  <div class="parallax-item" style="--paralaxy: {paralaxy}">
    <h2 class="section-title">Section 3</h2>
    <p>Keep scrolling to experience the smooth parallax transition on more sections!</p>
  </div>
  <div class="parallax-item" style="--paralaxy: {paralaxy}">
    <h2 class="section-title">Section 4</h2>
    <p>Final section with parallax. The effect should still be visible as you continue scrolling!</p>
  </div>
</div>
