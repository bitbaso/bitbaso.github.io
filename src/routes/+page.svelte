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
  
    onMount(() => {
      canvas.width = window.innerWidth;
      canvas.height = window.innerHeight;
      ctx = canvas.getContext("2d");
  
      initPoints();
      animate();
  
      const resizeHandler = () => {
        canvas.width = window.innerWidth;
        canvas.height = window.innerHeight;
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
      display: block;
      width: 100%;
      height: 100%;
      position: absolute;
      top: 50px;
      left: 0;
    }
  
    :global(body, html) {
      margin: 0;
      padding: 0;
      overflow: hidden;
      height: 100%;
    }
  </style>

<canvas class="bg-gradient-to-r from-stone-900 to-slate-200" bind:this={canvas}></canvas>
<div class="absolute inset-0 flex flex-col items-center justify-center text-center">
    <div class="flex items-center">
        <div class=" rounded-xl  bg-gradient-to-b from-green-600 to-slate-700 pr-5 pl-5 pt-1 pb-1 font-bold text-6xl text-white">b</div>
        <h1 class="text-6xl font-bold text-white drop-shadow-lg">itbaso</h1>
    </div>    
    <h2 class="mt-4 pl-20">Open source solutions</h2>
    <div class="mt-2 flex items-center">
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

  