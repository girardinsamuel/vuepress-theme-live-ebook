<template>
  <div class="chapter-blocks-wrapper">
    <ul
      class="chapter-blocks"
      :class="{
        'chapter-blocks--carousel': hasCarousel,
        'js-carousel': hasCarousel
      }"
    >
      <li
        v-for="(chapter, index) in chapters"
        :key="chapter.key"
        class="chapter"
      >
        <ChapterBlock
          :chapter-data="chapter"
          :order="index"
          :has-carousel="hasCarousel"
          :limit-section-in-chapter="limitSectionInChapter"
        />
      </li>
    </ul>

    <div
      v-if="hasCarousel"
      class="carousel-buttons"
      data-aos="fade-up"
      :data-aos-duration="aosAnimationDuration"
      :data-aos-offset="aosAnimationTriggerOffset"
    >
      <button
        class="carousel-button carousel-button--prev"
        aria-label="Previous slide"
        @click="goPrevSlide"
      >
        <ArrowLeft />
      </button>

      <button
        class="carousel-button carousel-button--next"
        aria-label="Next slide"
        @click="goNextSlide"
      >
        <ArrowRight />
      </button>
    </div>
  </div>
</template>

<script>
import Siema from 'siema'
import ChapterBlock from './ChapterBlock'
import ArrowRight from '@theme/assets/svg/arrow-right.svg'
import ArrowLeft from '@theme/assets/svg/arrow-left.svg'

export default {
  components: {
    ChapterBlock,
    ArrowRight,
    ArrowLeft,
  },

  props: {
    chapters: {
      type: Array,
      required: true,
    },
    limitSectionInChapter: {
      type: Number,
      default: 0,
    },
    hasCarousel: {
      type: Boolean,
      default: false,
    },
  },

  data () {
    return {
      aosAnimationDuration: 1000,
      aosAnimationTriggerOffset: -200,
      defaultCarouselSettings: {
        selector: '.js-carousel',
        duration: 300,
        easing: 'ease',
        loop: true,
        perPage: {
          992: 3,
        },
      },
    }
  },

  mounted () {
    if (this.hasCarousel) {
      this.initCarousel()
    }
  },

  beforeDestroy () {
    if (this.hasCarousel) {
      this.destroyCarousel()
    }
  },

  methods: {
    initCarousel () {
      this.carousel = new Siema(this.defaultCarouselSettings)
    },

    destroyCarousel () {
      const restoreOriginalMarkup = true
      this.carousel.destroy(restoreOriginalMarkup)
    },

    goNextSlide (index) {
      this.carousel.next()
    },

    goPrevSlide (index) {
      this.carousel.prev()
    },
  },
}
</script>

<style scoped lang="stylus">
  @require '~@theme/styles/shared'
  $c-shadow = rgb(86,75,65)

.chapter-blocks-wrapper
  position relative

.chapter-blocks
  display flex
  flex-direction column
  padding 0 36px

  &--carousel
    display block
    padding 40px 0 0

  @media (min-width: $screen-md-min)
    flex-direction row
    padding 0

.chapter
  flex 1
  padding 0
  margin-bottom 48px

  &::before
    content none

  @media (min-width: $screen-md-min)
    margin-bottom 0
    margin-left 30px

    &:first-of-type
      margin-left 0

.carousel-buttons
  position absolute
  top 0
  right 15px
  display flex
  z-index 50

  @media (min-width: $screen-md-min)
    top auto
    bottom 0

.carousel-button
  width 30px
  height 40px
  margin-left 10px
  color $c-primary
  cursor pointer
  transition transform ease-in-out .2s

  &--next
    &:hover
      transform: translateX(3px)

  &--prev
    &:hover
      transform: translateX(-3px)
</style>
