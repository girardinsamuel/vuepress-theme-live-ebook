<template>
  <div
    data-aos="fade-up"
    :data-aos-duration="aosAnimationDuration"
    :data-aos-offset="aosAnimationTriggerOffset"
    :data-aos-delay="aosDelayStartingValue * (order + 1)"
    class="chapter-wrapper"
    :class="{'chapter-wrapper--with-carousel': hasCarousel}"
  >
    <RouterLink
      class="chapter-content"
      :class="{'chapter-content--smaller-shadow': hasCarousel}"
      :to="chapterData.path"
    >
      <div class="chapter-header">
        <span class="header__number">
          {{ chapterData.frontmatter.chapter_number }}
        </span>
        <div class="header__arrow">
          <ArrowRight />
        </div>
      </div>
      <h3 class="chapter-title">
        {{ chapterData.frontmatter.title }}
      </h3>
      <ul class="chapter-sections">
        <li
          v-for="section in sections(chapterData)"
          :key="section.key"
          class="chapter-section"
        >
          {{ section.title }}
        </li>
      </ul>
    </RouterLink>
  </div>
</template>

<script>
import ArrowRight from '@theme/assets/svg/landing-arrow-right.svg'

export default {
  components: {
    ArrowRight,
  },

  props: {
    order: {
      type: Number,
      required: true,
    },
    hasCarousel: {
      type: Boolean,
      default: false,
    },
    limitSectionInChapter: {
      type: Number,
      default: 0,
    },
    chapterData: {
      type: Object,
      required: true,
      default: () => ({
        headers: [
          {
            level: null,
            slug: '',
            title: '',
          },
        ],
        path: '',
        frontmatter: () => ({
          chapter_number: '',
          description: '',
          layout: '',
          title: '',
        }),
      }),
    },
  },

  data () {
    return {
      aosAnimationDuration: 1000,
      aosAnimationTriggerOffset: -200,
      aosDelayStartingValue: 200,
    }
  },

  computed: {
    sections () {
      return chapter => {
        const chapterSections = chapter.headers && chapter.headers.filter(chapter => chapter.level === 2)

        return this.limitSectionInChapter !== 0 && chapterSections
          ? chapterSections.slice(0, this.limitSectionInChapter)
          : chapterSections
      }
    },
  },
}
</script>

<style scoped lang="stylus">
  @require '~@theme/styles/shared'
  $c-shadow = rgb(86,75,65)

.chapter-wrapper
  height 100%

  &--with-carousel
    padding 20px 15px 60px

.chapter__section
  padding 0

  &::before
    content none

.chapter-content
  display flex
  flex-direction column
  height 100%
  padding: 38px 30px 58px 30px
  background $c-white
  border-radius: 4px
  color: $c-landing-gray
  font-size $fs-4
  box-shadow 0 10px 20px 0 rgba($c-shadow, 0.05)
  transition: box-shadow ease .4s

  &:hover
    box-shadow 0 20px 40px 0 rgba($c-shadow, 0.3)

    .header__number,
    .header__arrow
      color $c-primary
      opacity 1

    .chapter-title
      color: $c-landing-text

  &--smaller-shadow:hover
    box-shadow 0 10px 20px 0 rgba($c-shadow, 0.3)

.chapter-header
  display flex
  justify-content space-between

.chapter-title
  margin 0
  font-weight $fw-semibold
  line-height 1.5
  transition: color ease .2s

.header__number
  margin-bottom 20px
  font-weight $fw-bold
  line-height 1
  opacity 0.5
  transition: opacity ease .2s, color ease .2s

.header__arrow
  width 20px
  height 14px
  color $c-primary
  opacity 0
  transition opacity ease .2s

.chapter-section
  margin-top 20px
  color $c-landing-gray
  font-size $fs-2
  line-height 1.5
  transition: color ease .2s

</style>
