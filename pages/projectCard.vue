<template>
  <div class="tw-flex tw-flex-col  tw-justify-center tw-items-center tw-my-5">
    <div class="tw-flex tw-flex-row">

    <!-- prev slide button -->
    <button @click="prev">
      <svg
        xmlns="http://www.w3.org/2000/svg"
        fill="none"
        viewBox="0 0 24 24"
        stroke-width="1.5"
        stroke="currentColor"
        class="tw-size-6"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          d="M6.75 15.75 3 12m0 0 3.75-3.75M3 12h18"
        />
      </svg>
    </button>

    <!-- visible part carousel  -->
    <div
      class="tw-carousel tw-flex tw-justify-center tw-items-center tw-w-[160vh] tw-mr-5 tw-ml-5 tw-overflow-hidden max-sm:tw-w-[30vh] 2xl:tw-w-[150vh]"
      ref="carousel"
    >
      <!-- whole carousel section -->
      <div
        class="tw-inner tw-w-[35%] tw-transition-transform tw-duration-300 tw-whitespace-nowrap max-sm:tw-w-[90%] sm:tw-w-[25%] lg:tw-w-[30%] lg:tw-mr-2 xl:tw-w-[25%] 2xl:tw-w-[95%]"
        ref="inner"
        :style="innerStyles"
      >
        <!-- carousel card section  -->
        <div class="tw-inline-flex" v-for="card in cards" :key="card.id">
          <div
            class="tw-card tw-flex tw-flex-col tw-flex-wrap tw-items-center tw-justify-between tw-mr-10 tw-self-stretch tw-w-[45vh] tw-h-[60vh] tw-pb-5 tw-pt-10 tw-px-10 tw-border tw-border-solid tw-border-stone-900 max-sm:tw-w-[26vh] max-sm:tw-px-0 max-sm:tw-h-[50vh] max-sm:tw-mr-12"
          >
            <div
              class="tw-w-full tw-self-center tw-break-words tw-whitespace-normal tw-mb-10 max-sm:tw-w-[24vh] max-sm:tw-mx-2 max-sm:tw-text-xs sm:tw-text-xs"
            >
              <h1 class="tw-mb-2 tw-font-bold">{{ card.title }}</h1>
              <h3>
                {{ card.subTitle }}
              </h3>
            </div>

            <img
              class="tw-w-[20vh] max-sm:tw-w-[10vh] max-sm:tw-mb-10 sm:tw-w-[15vh]"
              :src="card.image"
              alt="#"
            />
            <button
              class="tw-bg-violet-500 tw-text-white tw-text-md tw-px-11 tw-py-2 max-sm:tw-px-5"
            >
              Discover
            </button>
          </div>
        </div>
      </div>
    </div>

    <!-- next slide button -->
    <button @click="next">
      <svg
        xmlns="http://www.w3.org/2000/svg"
        fill="none"
        viewBox="0 0 24 24"
        stroke-width="1.5"
        stroke="currentColor"
        class="tw-size-6"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          d="M17.25 8.25 21 12m0 0-3.75 3.75M21 12H3"
        />
      </svg>
    </button>

    </div>


    <!-- Bullets -->
    <ul class="tw-flex tw-mt-5 tw-justify-center tw-w-full">
      <li
        v-for="(card, index) in cards"
        :key="card.id"
        :class="{
          'tw-bg-gray-700  , tw-rotate-45': currentIndex === index,
          'tw-bg-gray-300 ': currentIndex !== index,
        }"
        class="tw-w-2 tw-h-2 tw-duration-300 tw-mx-1 tw-rounded-sm tw-select-none"
      ></li>
    </ul>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";

// variables
const carousel = ref(null);
const inner = ref(null);
const innerStyles = ref({});
let step = "";
let transitioning = false;
let currentIndex = ref(0);

//carousel slide
// add a child to add another slide //
const cards = [
  {
    id: 1,
    image: "/img/Hero.png",
    title: "EmpowHER",
    subTitle:
      "Our mission is to empower women facing employment challenges,helping them secure jobs and achieve independence.",
  },
  {
    id: 2,
    image: "/img/Journal.png",
    title: "JournHEALism",
    subTitle:
      "The newspaper of our center, written by the same women we assist day after day. Heal through writing.",
  },
  {
    id: 3,
    image: "/img/Lighthouse.png",
    title: "SEAfer",
    subTitle:
      "Sail with us and forget all your worries as you navigate through the waves. Feel safer at sea.",
  },
  {
    id: 3,
    image: "/img/Lighthouse.png",
    title: "SEAfer",
    subTitle:
      "Sail with us and forget all your worries as you navigate through the waves. Feel safer at sea.",
  },
  {
    id: 3,
    image: "/img/Lighthouse.png",
    title: "SEAfer",
    subTitle:
      "Sail with us and forget all your worries as you navigate through the waves. Feel safer at sea.",
  },
];

// call this function before run
onMounted(() => {
  setStep();
  resetTranslate();
});

// function to Measure step size
const setStep = () => {
  const innerWidth = inner.value.scrollWidth;
  const totalCards = cards.length;
  step = `${innerWidth / totalCards}px`;
};

// function to next slide
const next = () => {
  if (transitioning) return;

  transitioning = true;

  //bullet next
  if (currentIndex.value < cards.length - 1) {
    currentIndex.value++;
  } else {
    currentIndex.value = 0;
  }

  // run move left animation
  moveLeft();

  // function to push next slide
  afterTransition(() => {
    const card = cards.shift();
    cards.push(card);
    resetTranslate();
    transitioning = false;
  });
};
// function to prev slide
const prev = () => {
  if (transitioning) return;

  transitioning = true;

  //bullet prev
  if (currentIndex.value > 0) {
    currentIndex.value--;
  } else {
    currentIndex.value = cards.length - 1;
  }
  // run move right animation
  moveRight();

  // function to remove prev slide
  afterTransition(() => {
    const card = cards.pop();
    cards.unshift(card);
    resetTranslate();
    transitioning = false;
  });
};

// move left animation
const moveLeft = () => {
  innerStyles.value = {
    transform: `translateX(-${step}) translateX(-${step})`,
  };
};
// move right animation
const moveRight = () => {
  innerStyles.value = {
    transform: `translateX(${step}) translateX(-${step})`,
  };
};

//add animation
const afterTransition = (callback) => {
  const listener = () => {
    callback();
    inner.value.removeEventListener("transitionend", listener);
  };
  inner.value.addEventListener("transitionend", listener);
};

//remove animation
const resetTranslate = () => {
  innerStyles.value = {
    transition: "none",
    transform: `translateX(-${step})`,
  };
};
</script>
