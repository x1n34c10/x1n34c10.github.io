---
layout: splash
title: "Información del autor"
permalink: /whoami/
date: 2021-06-11
---

<head>
<style>
  /*Inspired by Kevin Powell,
must use monospace font*/

:root {
  --number-of-characters: 21;
}
body {
  font-family: 'Roboto Mono', monospace;
  display: -ms-grid;
  display: grid;
  place-content: center;
  height: 100vh;
}
h1 {
  position: relative;
  display: inline-block;
}
h1::before, h1::after {
  position: absolute;
  content: '';
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
}
h1::before {
  background: white;
  opacity: 1;
  -webkit-animation: typewriter 4s steps(var(--number-of-characters)) 2s forwards;
          animation: typewriter 4s steps(var(--number-of-characters)) 2s forwards;
}
h1::after {
  width: 3px;
  background: black;
  -webkit-animation: typewriter 4s steps(var(--number-of-characters)) 2s forwards,
    blink 1.3s steps(var(--number-of-characters)) infinite;
          animation: typewriter 4s steps(var(--number-of-characters)) 2s forwards,
    blink 1.3s steps(var(--number-of-characters)) infinite;
}
@-webkit-keyframes typewriter {
  to {
    left: 100%;
  }
}
@keyframes typewriter {
  to {
    left: 100%;
  }
}
@-webkit-keyframes blink {
  0%  {background: black;}
  49%  {background: black;}
  50%  {background: transparent;}
  100%  {background: transparent;}
}
@keyframes blink {
  0%  {background: black;}
  49%  {background: black;}
  50%  {background: transparent;}
  100%  {background: transparent;}
} 
  </style>
<h1>Hi I'm WackyH4cker and I like computers.</h1>
</head>
