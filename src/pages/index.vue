<template>
  <div ref="el" h-full w-full overflow="y-scroll" :class="{ 'grid place-content-center': isStack }">
    <TransitionGroup tag="div" m="4" :class="isStack ? 'stack' : 'grid gap-4 grid-cols-2 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-5 xl:grid-cols-6 2xl:grid-cols-8'" name="fade">
      <div v-for="(item, index) in data" :key="index">
        <div class="card image-full max-w-60" bg-base-100 shadow-md>
          <figure><img src="https://placeimg.com/400/225/arch" alt="Shoes"></figure>
          <div class="card-body">
            <h2 class="card-title">
              Shoes!
            </h2>
            <p>If a dog chews shoes whose shoes does he choose?</p>
            <div class="card-actions justify-end">
              <button class="btn btn-primary">
                Buy Now
              </button>
            </div>
          </div>
        </div>
      </div>
    </TransitionGroup>
  </div>
</template>

<script setup lang="ts">
// interface Props {
//
// }
// const props = defineProps<Props>();

const el = ref<HTMLElement>()
const data = ref([1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18])
const onLoadMore = () => {
  const length = data.value.length + 1
  data.value.push(...Array.from({ length: 5 }, (_, i) => length + i))
}
useInfiniteScroll(
  el,
  onLoadMore,
  { distance: 10 },
)
const [isStack, toggleStack] = useToggle(true)

onMounted(async () => {
  const imgPromises = Array
    .from(document.querySelectorAll('img'))
    .map(img => new Promise((resolve, reject) => {
      img.onload = () => resolve(img)
      img.onerror = e => reject(e)
    }))
  await Promise.all(imgPromises)
  toggleStack()
})
</script>

<style scoped>
/* 1. 声明过渡效果 */
.fade-move,
.fade-enter-active,
.fade-leave-active {
  transition: all 0.5s cubic-bezier(0.55, 0, 0.1, 1);
}

/* 2. 声明进入和离开的状态 */
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
  transform: scaleY(0.01) translate(30px, 0);
}

/* 3. 确保离开的项目被移除出了布局流
      以便正确地计算移动时的动画效果。 */
.fade-leave-active {
  position: absolute;
}
</style>
