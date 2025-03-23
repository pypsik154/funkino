<template>
  <div class="nav-component">
    <!-- Мобильное меню -->
    <MobileMenu v-if="isMobile" :links="navLinks" @toggleSearch="toggleModalSearch" />

    <!-- Десктопная боковая панель -->
    <DesktopMenu v-else :links="navLinks" @toggleSearch="toggleModalSearch" />

    <!-- Модальное окно поиска -->
    <transition name="fade">
      <ModalSearch v-if="isModalSearchVisible" @closeModal="toggleModalSearch" />
    </transition>
  </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'

import MobileMenu from './MenuNavigation/MobileMenu.vue'
import DesktopMenu from './MenuNavigation/DesktopMenu.vue'
import ModalSearch from './ModalSearch.vue'

// Определяем, мобильное ли устройство (ширина окна меньше 600px)
const isMobile = ref(window.innerWidth < 601)

// Массив навигационных ссылок
const navLinks = [
  { to: '/', exact: true, icon: 'fas fa-home', text: 'Главная' },
  { to: '/top', icon: 'fa-solid fa-trophy', text: 'Популярное' },
  {
    href: 'https://github.com/pypsik154/FunKino-extensions/releases/tag/1.0',
    icon: 'fa-solid fa-puzzle-piece',
    text: 'Расширение'
  },
  { href: 'https://github.com/pypsik154/FunKino-extensions', icon: 'fab fa-github', text: 'Github' },
  {
    href: 'http://45.136.199.126:3001/status/reyohoho',
    icon: 'fas fa-tachometer-alt',
    text: 'Статус'
  },
  { to: '/setting', icon: 'fa-solid fa-gear', text: 'Настройки' }
]

// Обновляем состояние мобильного устройства при изменении размера окна
const updateIsMobile = () => {
  isMobile.value = window.innerWidth < 601
}

// Поиск в модальном окне
const isModalSearchVisible = ref(false)
const toggleModalSearch = () => {
  isModalSearchVisible.value = !isModalSearchVisible.value
}

// Добавляем и удаляем обработчики событий при монтировании/размонтировании компонента
onMounted(() => {
  window.addEventListener('resize', updateIsMobile)
})

onBeforeUnmount(() => {
  window.removeEventListener('resize', updateIsMobile)
})
</script>

<style scoped>
.nav-component {
  font-family: 'Neucha', sans-serif;
  font-weight: 400;
  font-size: 20px;
}

/* Стили для анимации fade */
.fade-enter-active {
  transition: opacity 0.3s ease;
}

.fade-leave-active {
  transition: all 0s;
}

.fade-enter-from {
  opacity: 0;
}

.fade-enter-to {
  opacity: 1;
}
</style>
