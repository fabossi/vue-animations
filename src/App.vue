<template>
  <div class="container">
    <div class="row">
      <div class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3">
        <h1>Animations</h1>
        <hr />
        <select v-model="alertAnimation" class="form-control">
          <option value="fade">Fade</option>
          <option value="slide">Slide</option>
        </select>
        <br />
        <button class="btn btn-primary" @click="show = !show">Show Alert</button>
        <br />
        <br />
        <transition :name="alertAnimation">
          <div class="alert alert-info" v-if="show">This is some Info</div>
        </transition>
        <transition :name="alertAnimation" type="animation">
          <div class="alert alert-info" v-if="show">This is some slide info</div>
        </transition>
        <transition
          enter-active-class="animated bouce"
          leave-active-class="animated shake"
          mode="out-in"
        >
          <div class="alert alert-info" v-if="show" key="info">This is some slide info</div>
          <div class="alert alert-warning" v-else key="warn">This is some slide warn</div>
        </transition>
        <hr />
        <button class="btn btn-primary" @click="load = !load">Load / remove element</button>
        <br />
        <br />
        <transition
          @before-enter="beforeEnter"
          @enter="enter"
          @after-enter="afterEnter"
          @enter-cancelled="enterCancelled"
          @before-leave="beforeLeave"
          @leave="leave"
          @after-leave="afterLeave"
          @leave-cancelled="leaveCancelled"
          :css="false"
        >
          <div style="width: 300px; height: 100px; background-color: lightgreen" v-if="load"></div>
        </transition>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      show: false,
      alertAnimation: "fade",
      load: true,
      elWidth: 100
    };
  },
  methods: {
    beforeEnter: el => {
      console.log("before enter");
      this.elWidth = 100;
      el.style.width = this.elWidth + "px";
    },
    enter: (el, done) => {
      console.log("enter");
      let round = 1;
      const interval = setInterval(() => {
        el.style.width = this.elWidth + round * 10 + "px";
        round++;
        if (round > 20) {
          clearInterval(interval);
          done();
        }
      }, 20);
    },
    afterEnter: el => {
      console.log("after enter");
    },
    enterCancelled: el => {
      console.log("enter cancelled");
    },
    beforeLeave: el => {
      console.log("before leave");
      this.elWidth = 300;
      el.style.width = this.elWidth + "px";
    },
    leave: (el, done) => {
      console.log("leave");
      let round = 1;
      const interval = setInterval(() => {
        el.style.width = this.elWidth - round * 10 + "px";
        round++;
        if (round > 20) {
          clearInterval(interval);
          done();
        }
      }, 20);
    },
    afterLeave: el => {
      console.log("after leave");
    },
    leaveCancelled: el => {
      console.log("leave cancelled");
    }
  }
};
</script>
 
<style>
.fade-enter {
  opacity: 0;
}

.fade-enter-active {
  transition: opacity 1s;
}

.fade-leave {
}

.fade-leave-active {
  transition: opacity 1s;
  opacity: 0;
}

.slide-enter {
  /* transform: translateY(20px); */
}

.slide-enter-active {
  animation: slide-in 1s ease-out forwards;
  transition: opacity 0.3s;
}

.slide-leave {
}

.slide-leave-active {
  animation: slide-out 1s ease-out forwards;
  transition: opacity 0.3s;
}

@keyframes slide-in {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    transform: translateY(0);
    opacity: 1;
  }
}

@keyframes slide-out {
  from {
    opacity: 1;
    transform: translateY(0);
  }
  to {
    opacity: 0;
    transform: translateY(20px);
  }
}
</style>
