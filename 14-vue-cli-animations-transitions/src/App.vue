<template>
  <div class="container">
    <div class="row">
      <div class="col-md-6 col-md-offset-3">
        <h3>Animation ve Transition</h3>
        <hr />

        <!-- <select class="form-control" v-model="activeEffect">
          <option value="fade">fade</option>
          <option value="slide">slide</option>
        </select>

        <button class="btn btn-primary" @click="show = !show">
          kutuyu göster/gizle
        </button>
        <br /><br />
              <transition :name="activeEffect">
          <div class="alert alert-success" v-if="show">
            bu bir alert kutusudur
          </div>
        </transition>
        <hr />

        <transition name="slide" type="animation" appear>
          <div class="alert alert-warning" v-if="!show">
            bu bir alert kutusudur
          </div>
        </transition>
        <hr />
        <hr />
        <transition
          enter-class=""
          enter-active-class="animate__animated animate__bounce"
          leave-class=""
          leave-active-class="animate__animated animate__swing"
          appear
        >
          <div class="alert alert-warning" v-if="!show">
            bu bir alert kutusudur
          </div>
        </transition>
        <hr />

        <br /><br />
        <transition name="fade" mode="out-in">
          <div class="alert alert-success" key="success" v-if="show">
            bu bir alert kutusudur
          </div>
          <div class="alert alert-danger" key="danger" v-else>
            bu bir alert kutusudur
          </div>
        </transition>
        -->

        <button class="btn btn-primary" @click="showJs = !showJs">
          kutuyu göster/gizle
        </button>
        <hr />

        <!--:css="false" kullanmazsak default oalrak css classlarını arar (v-enter gibi) -->
        <transition
          :css="false"
          @before-enter="beforeEnter"
          @enter="enter"
          @after-enter="afterEnter"
          @after-enter-cancelled="afterEnterCancelled"
          @before-leave="beforeLeave"
          @leave="leave"
          @after-leave="afterLeave"
          @after-leave-cancelled="afterLeaveCancelled"
        >
          <div
            style="width:100px; height:100px; background-color:#fbbd08; border: 1px solid #666;"
            key="success"
            v-if="showJs"
          >
            bu bir alert kutusudur
          </div>
        </transition>

        <h3>dinamik komponenetler arası geçiş</h3>
        <hr />
        <button class="btn btn-danger" @click="activeComponent = 'app-home'">
          home
        </button>
        <button class="btn btn-primary" @click="activeComponent = 'app-post'">
          post
        </button>
        <transition name="fade" mode="out-in">
          <component :is="activeComponent"></component>
        </transition>
        <hr />
        <br /><br />

        <button class="btn btn-danger" @click="addNewItem">
          yeni eleman ekle
        </button>
        <ul class="list-group">
          <transition-group name="slide">
            <li
              v-for="(number, index) in numberList"
              :key="number"
              @click="removeItem(index)"
              class="list-group-item"
            >
              Number : {{ number }}
            </li>
          </transition-group>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import Post from "./components/Post";
import Home from "./components/Home";

export default {
  components: {
    "app-post": Post,
    "app-home": Home,
  },
  data() {
    return {
      show: false,
      showJs: false,
      activeEffect: "fade",
      elementWidth: 100,
      activeComponent: "app-post",
      numberList: [1, 2, 3, 4, 5],
    };
  },
  methods: {
    addNewItem() {
      const position = Math.floor(Math.random() * this.numberList.length);
      this.numberList.splice(position, 0, this.numberList.length + 1);
    },
    removeItem(index) {
      this.numberList.splice(index, 1);
    },
    beforeEnter(el) {
      console.log("beforeEnter");
      this.elementWidth = 100;
      el.style.width = this.elementWidth + "px";
    },
    enter(el, done) {
      console.log("enter");
      let round = 1;
      const interval = setInterval(() => {
        el.style.width = this.elementWidth + round * 10 + "px";
        round++;
        if (round == 20) {
          clearInterval(interval);
          done();
        }
      }, 50);
    },
    afterEnter(el) {
      console.log("afterEnter");
    },
    afterEnterCancelled(el) {
      console.log("afterEnterCancelled");
    },
    beforeLeave(el) {
      console.log("beforeLeave");
      this.elementWidth = 300;
      el.style.width = this.elementWidth + "px";
    },
    leave(el, done) {
      console.log("leave");

      let round = 1;
      const interval = setInterval(() => {
        el.style.width = this.elementWidth - round * 10 + "px";
        round++;
        if (round == 20) {
          clearInterval(interval);
          done();
        }
      }, 50);
    },
    afterLeave(el) {
      console.log("afterLeave");
    },
    afterLeaveCancelled(el) {
      console.log("afterLeaveCancelled");
    },
  },
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
  opacity: 0;
}
.slide-enter-active {
  animation: slide-in 1s ease-out forwards;
  transition: opacity 0.5s;
}

.slide-leave {
}
.slide-leave-active {
  animation: slide-out 1s ease-out forwards;
  transition: opacity 1s;
  opacity: 0;
  position: absolute;
}
.slide-move {
  transition: transform 1s;
}

@keyframes slide-in {
  from {
    transform: translateY(60px);
  }
  to {
    transform: translateY(0px);
  }
}

@keyframes slide-out {
  from {
    transform: translateY(0px);
  }
  to {
    transform: translateY(60px);
  }
}
</style>
