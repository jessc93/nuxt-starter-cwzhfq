<template>
  <main>
    <div id="smooth-wrapper" ref="{main}">
      <div id="smooth-content">
        <header class="header">
          <h1 class="title">GreenSock ScrollSmoother on a Nuxt2 App</h1>
          <button class="button" @click="scrollTo">Jump to C</button>
        </header>
        <div class="box box-a" data-speed="0.5">a</div>
        <div class="box box-b" data-speed="0.8">b</div>
        <div class="box box-c" data-speed="1.5">c</div>
        <div class="line"></div>
      </div>
    </div>
    <footer>
      <a href="https://greensock.com/scrollsmoother">
        <img
          alt="ScrollSmoother"
          class="greensock-icon"
          src="https://s3-us-west-2.amazonaws.com/s.cdpn.io/16327/scroll-smoother-logo-light.svg"
          width="220"
          height="70"
        />
      </a>
    </footer>
  </main>
</template>

<script>
import gsap from 'gsap-trial';
import { ScrollTrigger } from 'gsap-trial/ScrollTrigger';
import { ScrollSmoother } from 'gsap-trial/ScrollSmoother';

if (typeof window !== 'undefined') {
  gsap.registerPlugin(ScrollTrigger, ScrollSmoother);
}

export default {
  name: 'IndexPage',
  mounted() {
    this.smoother = null;
    this.ctx = gsap.context(() => {
      this.smoother = ScrollSmoother.create({
        smooth: 2, // seconds it takes to "catch up" to native scroll position
        effects: true, // look for data-speed and data-lag attributes on elements and animate accordingly
      });
      ScrollTrigger.create({
        trigger: '.box-c',
        pin: true,
        start: 'center center',
        end: '+=300',
        markers: true,
      });
    }, this.$refs.main);
  },
  beforeDestroy() {
    this.ctx.revert();
  },
  methods: {
    scrollTo() {
      this.smoother.scrollTo('.box-c', true, 'center center');
    },
  },
};
</script>
