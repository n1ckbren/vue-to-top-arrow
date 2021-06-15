<template>
  <div @click="clickToTop" class="vue-to-top" ref="navElement">
    <svg class="vue-to-top-progress-circle svg-content" width="100%" height="100%" viewBox="-1 -1 102 102">
      <path d="M50,1 a49,49 0 0,1 0,98 a49,49 0 0,1 0,-98"/>
    </svg>
  </div>
</template>

<script>
  export default {
    data(){
      return {
        currentOffsetTop: null,
        pageHeight: null,
        clientHeight: null,
        progressPath: null,
        pathLength: null

      }
    },

    created() {
      window.addEventListener('scroll', this.handleScroll);
    },

    mounted() {
      this.clientHeight = document.documentElement.clientHeight;
      this.pageHeight = document.body.scrollHeight;
      this.progressPath = document.querySelector('.vue-to-top path');
      this.pathLength = this.progressPath.getTotalLength();
      this.progressPath.style.transition = this.progressPath.style.WebkitTransition = 'none';
      this.progressPath.style.strokeDasharray = this.pathLength + ' ' + this.pathLength;
      this.progressPath.style.strokeDashoffset = this.pathLength;
      this.progressPath.getBoundingClientRect();
      this.progressPath.style.transition = this.progressPath.style.WebkitTransition = 'stroke-dashoffset 10ms linear';		
      this.drawProgress();
    },

    destroyed() {
      window.removeEventListener('scroll', this.handleScroll);
    },

    methods: {
      handleScroll() {
        this.currentOffsetTop = window.pageYOffset;
        this.showToTop();
        this.drawProgress();
      },

      clickToTop(){
        window.scrollTo({ top: 0, behavior: 'smooth' });
      },

      showToTop(){
        if (this.currentOffsetTop > 50) {
          this.$refs.navElement.classList.add('active')
        } else {
          this.$refs.navElement.classList.remove('active')
        }
      },

      drawProgress() {
        let progress = this.pathLength - ((this.currentOffsetTop + this.clientHeight) * this.pathLength / this.pageHeight);
        this.progressPath.style.strokeDashoffset = progress;
      }
    }
  };
</script>

<style scoped>
  .vue-to-top {
    border-radius: 50px;
    bottom: 50px;
    box-shadow: inset  0 0 0 2px rgba(255, 255, 255, .3);
    cursor: pointer;
    display: block;
    height: 46px;
    opacity: 0;
    position: fixed;
    right: 50px;
    transform: translateY(15px);
    visibility: hidden;
    width: 46px;
    z-index: 10000;
    -webkit-transition: all 200ms linear;
      transition: all 200ms linear;
  }

  .vue-to-top.active {
    opacity: 1;
    transform: translateY(0);
    visibility: visible;
  }
  
  .vue-to-top::after {
    border-style: solid;
    border-width: 2px 2px 0 0;
    color: #000;
    content: '';
    cursor: pointer;
    display: inline-block;
    height: 11px;
    left: 16px;
    position: absolute;
    top: 19px;
    transform: rotate(-45deg);
    width: 11px;
    z-index: 1;
    -webkit-transition: all 200ms linear;
      transition: all 200ms linear;
  }

  .vue-to-top svg path { 
    fill: none; 
  }

  .vue-to-top svg.vue-to-top-progress-circle path {
    stroke: #000;
    stroke-width: 4;
    box-sizing:border-box;
    -webkit-transition: all 200ms linear;
      transition: all 200ms linear;
  }
</style>