<script setup>
import SplitType from 'split-type';

const { gsap } = useGsap();
const emitter = useEmitter();

const title = ref(null);

function showTitle() {
  const revealRef = gsap.timeline({
    defaults: { ease: 'expo.out' },
  });

  revealRef.to('.project-title__line__word', {
    yPercent: -105,
    stagger: 0.1,
    duration: 1.25,
  });

  revealRef.to(
    '.project-title__line__wrapper',
    { y: 0, stagger: 0.075, duration: 1 },
    0.1,
  );
}

onMounted(() => {
  const text = new SplitType(title.value, {
    types: 'lines, words',
    lineClass: 'project-title__line',
    wordClass: 'project-title__line__word',
  });

  for (const line of text.lines) {
    const lineParent = line.parentNode;
    line.remove();

    const wrapper = document.createElement('div');
    wrapper.classList.add('project-title__line__wrapper');

    lineParent.appendChild(wrapper);
    wrapper.appendChild(line);
  }
});

emitter.once('overlay:hiding', showTitle);
</script>

<template>
  <h1 ref="title" class="project-title" data-scroll data-scroll-speed="1">
    <ContentSlot :use="$slots.default" :unwrap="true" />
  </h1>
</template>

<style lang="scss">
.project-title {
  font-weight: 200;

  max-width: 60vw;

  color: var(--ff-color);

  margin: 0;
  margin-bottom: 0.75rem;

  &__line {
    line-height: 1.225;

    &__word {
      transform: translateY(105%);
    }

    &__wrapper {
      overflow: hidden;

      transform: translateY(40px);
    }
  }

  @media screen and (max-width: 900px) {
    max-width: 100%;
  }
}
</style>
