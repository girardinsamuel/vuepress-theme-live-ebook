<template>
  <section class="landing-chapters">
    <div class="landing-chapters-container">
      <div
        class="landing-chapters__col landing-chapters__col--intro"
        :class="{'landing-chapters__col--extra-padding': hasCarousel}"
      >
        <SectionIntro
          class="intro"
          :title="title"
          :subtitle="subtitle"
          :intro="intro"
          :animation-trigger-offset="-200"
        />
        <div
          v-if="img"
          class="image-wrapper"
        >
          <BaseImage
            class="image js-parallax"
            data-rellax-speed="-0.5"
            :img="img"
            :alt="imgAlt"
          />
        </div>
      </div>
      <div
        class="landing-chapters__col landing-chapters__col--chapters"
        :class="{'landing-chapters__col--with-carousel': hasCarousel}"
      >
        <ChapterBlocks
          :chapters="chapters"
          :limit-section-in-chapter="limitSectionInChapter"
          :has-carousel="hasCarousel"
        />
      </div>
      <ReadButton class="button" />
    </div>
  </section>
</template>

<script>

import BaseImage from '@theme/global-components/BaseImage'
import ReadButton from '@theme/components/ReadButton'
import ChapterBlocks from '@theme/components/ChapterBlocks'
import SectionIntro from '@theme/components/SectionIntro'

export default {
  components: {
    BaseImage,
    ReadButton,
    ChapterBlocks,
    SectionIntro,
  },
  props: {
    title: {
      type: String,
      required: false,
      default: 'Chapters',
    },
    subtitle: {
      type: String,
      required: false,
      default: 'MENU',
    },
    intro: {
      type: String,
      required: false,
      default: null,
    },
    img: {
      type: String,
      required: false,
      default: null,
    },
    imgAlt: {
      type: String,
      default: '',
    },
    chapterDisplayStart: {
      type: Number,
      default: 0,
    },
    limitSectionInChapter: {
      type: Number,
      default: undefined,
    },
    hasCarousel: {
      type: Boolean,
      default: false,
    },
  },

  computed: {
    chapterDisplayEnd () {
      return this.chapterDisplayStart + 3
    },

    chapters () {
      const chapters = this.hasCarousel
        ? this.$chapters.slice(this.chapterDisplayStart)
        : this.$chapters.slice(this.chapterDisplayStart, this.chapterDisplayEnd)

      return chapters
    },
  },
}
</script>

<style scoped lang="stylus">
  @require '~@theme/styles/shared'

.landing-chapters
  padding 94px 0 50px
  background: $c-white

  @media (min-width: $screen-md-min)
    padding 180px 0 140px

.landing-chapters-container
  @extend $landing-container
  display flex
  flex-direction row
  justify-content center
  align-items flex-start
  flex-wrap wrap

.landing-chapters__col
  flex-basis: 100%

  &--intro
    flex 1
    margin-bottom 80px

  &--extra-padding
    @media (min-width: $screen-md-min)
      padding-top 20px

  &--with-carousel
    max-width 880px
    overflow hidden

    @media (min-width: $screen-md-min)
      max-width none

  @media (min-width: $screen-md-min)
    &--intro
      flex 1
      margin-bottom 0

    &--chapters
      flex 3
      position relative
      z-index 20

.image-wrapper
  position relative
  width calc(100% - 65px)
  left -15px

.image
  display none

  @media (min-width: $screen-md-min)
    display block
    margin 50px 0 0

.button
  margin-top 5px

  @media (min-width: $screen-sm-min)
    display none

.intro
  max-width 270px
  text-align center
  margin: auto

  @media (min-width: $screen-md-min)
    max-width 166px
    margin-left 0
    text-align left

</style>
