<script>
  import { onMount, tick } from "svelte";
  import { assets } from '$app/paths';

  let canvas;
  let ctx;
  let points = [];
  const maxPoints = 100;

  class Point {
    constructor(x, y, index) {
      this.x = x;
      this.y = y;
      this.vx = Math.random() * 2 - 1;
      this.vy = Math.random() * 2 - 1;
      this.index = index; // Índice único para alternar colores
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

      // Alternar entre rojo, negro y verde según el índice
      if (this.index % 3 === 0) {
        ctx.fillStyle = "#ff0000"; // Rojo
      } else if (this.index % 3 === 1) {
        ctx.fillStyle = "#000000"; // Negro
      } else {
        ctx.fillStyle = "#00ff00"; // Verde
      }

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
          ctx.strokeStyle = `rgba(255, 255, 255, ${1 - distance / 100})`; // Color blanco
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
      points.push(new Point(x, y, i)); // Pasar índice único a cada punto
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
  let descriptionText = "";
  let i = 0;

  const delay = 100; // Tiempo en milisegundos entre cada letra

  const type = async () => {
    while (i < text.length) {
      await new Promise(resolve => setTimeout(resolve, delay)); // Espera antes de escribir la siguiente letra
      descriptionText += text[i++];
    }
  };

  $: type(); // Inicia la escritura cuando el componente se monta

  let quotes = [
    "“Programs must be written for people to read, and only incidentally for machines to execute.” – Harold Abelson",
    "“Any fool can write code that a computer can understand. Good programmers write code that humans can understand.” – Martin Fowler",
    "“Code is like humor. When you have to explain it, it’s bad.” – Cory House",
    "“First, solve the problem. Then, write the code.” – John Johnson",
    "“Clean code always looks like it was written by someone who cares.” – Robert C. Martin",
    "“Make it work, make it right, make it fast.” – Kent Beck",
    "“The KISS principle reminds us to aim for simplicity in our code, avoiding unnecessary complexity.” – Anonymous",
    "“SOLID principles guide us to write code that is scalable, maintainable, and easy to understand.” – Robert C. Martin"
];
    
let randomQuote = quotes[0];
let fade = false;

const updateQuote = () => {
  fade = true; // Activar animación de salida
  setTimeout(() => {
    randomQuote = quotes[Math.floor(Math.random() * quotes.length)];
    fade = false; // Activar animación de entrada
  }, 500); // Sincronizar con la duración de la animación
};

  onMount(() => {
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight; // Altura del canvas a 30% de la altura de la ventana
    ctx = canvas.getContext("2d");

    initPoints();
    animate();

    const resizeHandler = () => {
      canvas.width = window.innerWidth;
      canvas.height = window.innerHeight; // Reajustamos el canvas cuando se redimensiona
      initPoints();
    };

    window.addEventListener("resize", resizeHandler);

    const interval = setInterval(updateQuote, 10000); // Cambiar cita cada 10 segundos    

    return () => {
      clearInterval(interval); // Limpiar intervalo al desmontar el componente
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

  .quote-box {
    transition: opacity 0.5s ease-in-out; /* Efecto de transición suave */
    opacity: 1;
  }

  .quote-box.fade-out {
    opacity: 0; /* Desaparecer */
  }
</style>

<canvas bind:this={canvas} class="bg-gradient-to-b from-stone-900 to-slate-500"></canvas>

<!-- Contenedor de texto con posición relativa, que se coloca sobre el canvas -->
<div class="relative z-10 flex flex-col items-center justify-center mt-64 mb-64 pb-6">
  <div class="flex items-center justify-center">    
    <h1 class="text-6xl font-bold text-white drop-shadow-lg">bitbaso</h1>
  </div>    
  
  <div class="mt-2 flex items-center">
    <div class=" pr-2">
      <h2 class="pl-20 text-white">{descriptionText}</h2>
    </div>
    <div>
      <a href="https://github.com/bitbaso" target="_blank" title="github bitbaso">
          <img src={`${assets}/github-mark-white.png`} width="20px" alt="bitbaso github" />
      </a>    
  </div>
  </div>
</div>

<div class="h-full w-full bg-gray-400 rounded-md bg-clip-padding 
backdrop-filter backdrop-blur-sm bg-opacity-60 border border-gray-100">
  {#if randomQuote}
    <div class="quote-box mt-4 mb-4 m-2 text-white font-semibold {fade ? 'fade-out' : ''}">
      {randomQuote}
    </div>
  {/if}
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
