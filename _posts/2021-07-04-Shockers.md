---
title: "HackTheBox - ArcheType"
layout: single
excerpt: En esta ocasión traigo el Write Up de la máquina ArcheType de HackTheBox, a mi parecer es una máquina notable para personas que están empezando en Directorio Activo y el hacking en Windows.
header:
show_date: true
classes: wide
header:
  teaser: "https://user-images.githubusercontent.com/69093629/124369575-53c69000-dc6d-11eb-83db-4b4caea04ddc.png"
  teaser_home_page: true
  icon: "https://user-images.githubusercontent.com/69093629/124369575-53c69000-dc6d-11eb-83db-4b4caea04ddc.png"
categories:
  - HackTheBox
tags:
  - Write Up
  - Starting Point
---

<head>
<style>
  html,
body {
  width: 100%;
  height: 100%;
  margin: 0;
  padding: 0;
  font-size: 20px;
}

*,
*:before,
*:after {
  box-sizing: border-box;
  position: relative;
}

body {
  background: #000;
  color: #fff;
  height: auto;
  min-height: 100%;
  overflow: hidden;
}

main {
  background: #111;
  border: solid 1px #222;
  padding: 2rem;
  max-width: 100%;
  width: 960px;
  margin: 0 auto;
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-gap: 2rem;

  > * {
    grid-column: 1 / -1;
  }
}

img {
  max-width: 100%;
}

p {
  line-height: 1.8;
  margin: 1rem 0;
  color: rgb(158, 166, 184);
}

h1 {
  line-height: 1.3;
  font-size: 5vw;
  margin: 1rem 0;
}

h2 {
  font-size: 4vw;
  margin: 1rem 0;
}

h3 {
  font-size: 2vw;
  font-weight: bold;
}

h2.subheader {
  font-size: 2vw;
}

section {
  grid-column: auto;
}

header {
  display: grid;
  align-content: center;
  grid-column: 1 / -1;
}

.callout {
  text-align: center;
  background-color: #3173fa;

  > p {
    color: white;
  }

  padding: 1vw 3vw;
}

/* ---------------------------------- */

.container {
  perspective: 1200px;
  transform-style: preserve-3d;

  animation: cinematic-camera 11s cubic-bezier(0.6, 0, 0.4, 1) both infinite;
  @keyframes cinematic-camera {
    from {
      perspective-origin: 60% 40%;
    }
    to {
      perspective-origin: 40% 60%;
    }
    /* 
      Move the fading to the containing element as to not break inside 3D transforms. 
      See: https://css-tricks.com/things-watch-working-css-3d/
    */
    from,
    to {
      opacity: 0;
    }
    25%,
    75% {
      opacity: 1;
    }
  }

  &:after {
    content: "";
    background: linear-gradient(to bottom, #000, #0000 20%, #0000 80%, #000);
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100vh;
  }
}

main {
  transform-origin: top center;
  transform-style: preserve-3d;

  animation: inherit;
  animation-name: cinematic;

  // Fixed the 3D image transform.
  > img {
    display: block;
    transform-style: preserve-3d;
    animation: inherit;
    animation-name: image-pop;
    @keyframes image-pop {
      70%,
      100% {
        transform: translate3d(0, 0, 60px);
      }
    }

    &:last-of-type {
      animation-delay: 4s;
    }
  }

  @keyframes cinematic {
    from {
      transform: translateZ(-200px) rotateY(30deg) translateY(50vh);
    }
    to {
      transform: translateZ(-100px) rotateY(-30deg) translateY(-100%)
        translateY(50vh);
    }
  }
}
  </style>
  <div class="container">
  <main>
    <header>
      <h2 class="subheader">— Keyframers</h2>
      <h1>Where we bring imaginative user interfaces <em>to life.</em></h1>
      <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Rerum suscipit ipsam aspernatur quasi reiciendis at eum cupiditate officiis repudiandae quae ea facere odit beatae voluptate recusandae quas, possimus laborum inventore.</p>
    </header>

    <img src="https://images.unsplash.com/photo-1558459654-c430be5b0a44?crop=entropy&cs=tinysrgb&fit=crop&fm=jpg&ixid=MXwxNDU4OXwwfDF8cmFuZG9tfHx8fHx8fHw&ixlib=rb-1.2.1&q=80&w=960&h=500" alt="" />

    <section>
      <h2>The Client</h2>
      <p>You! Lorem ipsum dolor sit amet consectetur adipisicing elit. Totam voluptatum quidem eligendi debitis quia dignissimos ipsa error, atque quibusdam corrupti soluta facere nulla neque nostrum recusandae assumenda, aspernatur in. Provident!</p>
    </section>

    <section>

      <h2>Our Mission</h2>
      <p>To educate the world in web animation. Lorem ipsum dolor sit amet consectetur adipisicing elit. Blanditiis ipsa doloremque, natus dolorum a perferendis modi veritatis ab earum, culpa nemo, aliquam qui! Nostrum iste ullam voluptatem, doloribus odit autem.</p>
    </section>
    <div class="callout">
      <h3>Get animating!</h3>
      <p>Lorem ipsum, dolor sit amet consectetur adipisicing elit. Consequatur magni dolores iusto nulla vitae, reiciendis amet veritatis aliquam iste temporibus itaque aliquid, eveniet saepe reprehenderit distinctio eaque libero, culpa tenetur.</p>
    </div>

    <img src='https://images.unsplash.com/photo-1603791445824-0050bd436b6d?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&ixid=MXwxNDU4OXwwfDF8cmFuZG9tfHx8fHx8fHw&ixlib=rb-1.2.1&q=80&w=960' alt=''>
  </main>
</div>

<a href="https://youtu.be/cxwZfcoiUSE" target="_blank" data-keyframers-credit style="color: #FFF; z-index: 10"></a>
<script src="https://codepen.io/shshaw/pen/QmZYMG.js"></script>
</head>
