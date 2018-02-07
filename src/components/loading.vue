<template>
  <!-- animation -->
    <div class="self-indicator-wrapper">
      <span class="self-indicator-spin">
        <div class="self-spinner-fading-circle" style="width: 32px;height: 32px;">
          <div class="self-spinner-fading-circle-circle is-circle2"></div>
          <div class="self-spinner-fading-circle-circle is-circle3"></div>
          <div class="self-spinner-fading-circle-circle is-circle4"></div>
          <div class="self-spinner-fading-circle-circle is-circle5"></div>
          <div class="self-spinner-fading-circle-circle is-circle6"></div>
          <div class="self-spinner-fading-circle-circle is-circle7"></div>
          <div class="self-spinner-fading-circle-circle is-circle8"></div>
          <div class="self-spinner-fading-circle-circle is-circle9"></div>
          <div class="self-spinner-fading-circle-circle is-circle10"></div>
          <div class="self-spinner-fading-circle-circle is-circle11"></div>
          <div class="self-spinner-fading-circle-circle is-circle12"></div>
          <div class="self-spinner-fading-circle-circle is-circle13"></div>
        </div>
      </span>
    </div>
</template>

<style>
  .self-indicator-wrapper {
    margin: 0 auto;
    padding: 10px;
    color: #fff;
    box-sizing: border-box;
    text-align: center;
  }
  .self-indicator-spin {
    display: inline-block;
    text-align: center;
  }

  .self-spinner-fading-circle {
    position: relative;
  }
  .self-spinner-fading-circle > div::before {
    background-color: #000;
  }
  .self-spinner-fading-circle-circle {
    width: 100%;
    height: 100%;
    top: 0;
    left: 0;
    position: absolute;
  }
  .self-spinner-fading-circle-circle::before {
    content: " ";
    display: block;
    margin: 0 auto;
    width: 15%;
    height: 15%;
    border-radius: 100%;
    -webkit-animation: self-fading-circle 1.2s infinite ease-in-out both;
    animation: self-fading-circle 1.2s infinite ease-in-out both
  }
  .self-spinner-fading-circle-circle.is-circle2 {
    -webkit-transform: rotate(30deg);
    transform: rotate(30deg)
  }
  .self-spinner-fading-circle-circle.is-circle2::before {
    -webkit-animation-delay: -1.1s;
    animation-delay: -1.1s
  }
  .self-spinner-fading-circle-circle.is-circle3 {
    -webkit-transform: rotate(60deg);
    transform: rotate(60deg)
  }
  .self-spinner-fading-circle-circle.is-circle3::before {
    -webkit-animation-delay: -1s;
    animation-delay: -1s
  }
  .self-spinner-fading-circle-circle.is-circle4 {
    -webkit-transform: rotate(90deg);
    transform: rotate(90deg)
  }
  .self-spinner-fading-circle-circle.is-circle4::before {
    -webkit-animation-delay: -0.9s;
    animation-delay: -0.9s
  }
  .self-spinner-fading-circle-circle.is-circle5 {
    -webkit-transform: rotate(120deg);
    transform: rotate(120deg)
  }
  .self-spinner-fading-circle-circle.is-circle5::before {
    -webkit-animation-delay: -0.8s;
    animation-delay: -0.8s
  }
  .self-spinner-fading-circle-circle.is-circle6 {
    -webkit-transform: rotate(150deg);
    transform: rotate(150deg)
  }
  .self-spinner-fading-circle-circle.is-circle6::before {
    -webkit-animation-delay: -0.7s;
    animation-delay: -0.7s
  }
  .self-spinner-fading-circle-circle.is-circle7 {
    -webkit-transform: rotate(180deg);
    transform: rotate(180deg)
  }
  .self-spinner-fading-circle-circle.is-circle7::before {
    -webkit-animation-delay: -0.6s;
    animation-delay: -0.6s
  }
  .self-spinner-fading-circle-circle.is-circle8 {
    -webkit-transform: rotate(210deg);
    transform: rotate(210deg)
  }
  .self-spinner-fading-circle-circle.is-circle8::before {
    -webkit-animation-delay: -0.5s;
    animation-delay: -0.5s
  }
  .self-spinner-fading-circle-circle.is-circle9 {
    -webkit-transform: rotate(240deg);
    transform: rotate(240deg)
  }
  .self-spinner-fading-circle-circle.is-circle9::before {
    -webkit-animation-delay: -0.4s;
    animation-delay: -0.4s
  }
  .self-spinner-fading-circle-circle.is-circle10 {
    -webkit-transform: rotate(270deg);
    transform: rotate(270deg)
  }
  .self-spinner-fading-circle-circle.is-circle10::before {
    -webkit-animation-delay: -0.3s;
    animation-delay: -0.3s
  }
  .self-spinner-fading-circle-circle.is-circle11 {
    -webkit-transform: rotate(300deg);
    transform: rotate(300deg)
  }
  .self-spinner-fading-circle-circle.is-circle11::before {
    -webkit-animation-delay: -0.2s;
    animation-delay: -0.2s
  }
  .self-spinner-fading-circle-circle.is-circle12 {
    -webkit-transform: rotate(330deg);
    transform: rotate(330deg)
  }
  .self-spinner-fading-circle-circle.is-circle12::before {
    -webkit-animation-delay: -0.1s;
    animation-delay: -0.1s
  }
  @-webkit-keyframes self-fading-circle {
    0%, 39%, 100% {
      opacity: 0
    }
    40% {
      opacity: 1
    }
  }
  @keyframes self-fading-circle {
    0%, 39%, 100% {
      opacity: 0
    }
    40% {
      opacity: 1
    }
  }
</style>

