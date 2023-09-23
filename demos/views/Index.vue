<template>
  <el-container direction="vertical">
    <el-header class="header">
      <div class="header__left">
        <img src="~demos/assets/logo.svg" class="logo" />
      </div>

      <div class="header__right">
        <router-link
          v-for="{ label, route } in routes"
          :key="label"
          :to="{ name: route }"
          custom
          v-slot="{ navigate, isExactActive }"
        >
          <span
            :class="{ 'router-link--active': isExactActive }"
            class="router-link"
            @click="navigate"
          >
            {{ label }}
          </span>
        </router-link>
      </div>
    </el-header>

    <el-main>
      <router-view />
    </el-main>
  </el-container>
</template>

<script setup>
const routes = [
  {
    label: 'Simple Usage',
    route: 'Simple',
  },
  {
    label: 'All Extensions',
    route: 'AllExtensions',
  },
  {
    label: 'Bubble Menu',
    route: 'BubbleMenu',
  },
  {
    label: 'Placeholder',
    route: 'Placeholder',
  },
  {
    label: 'Readonly',
    route: 'Readonly',
  },
  {
    label: 'Title',
    route: 'Title',
  },
  {
    label: 'Event',
    route: 'Event',
  },
  {
    label: 'Output',
    route: 'Output',
  },
];
</script>

<style lang="scss">
.el-container {
  background: linear-gradient(to bottom, #2980b9, #6dd5fa, #ffffff);
  font-family: sans-serif;
}

.header {
  display: flex;
  justify-content: center;
  height: 20vh !important;
  padding-bottom: 20px;
  padding-top: 20px;

  &__left {
    align-items: flex-start;
    display: flex;
    flex-direction: column;
    margin-top: 50px;

    .logo {
      height: 50px;
      margin-bottom: 20px;
    }
  }

  &__right {
    align-items: flex-start;
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    margin-top: 50px;
    margin-left: 10vw;
    width: 650px;

    .router-link {
      color: #fff;
      cursor: pointer;
      flex-shrink: 0;
      font-weight: 700;
      letter-spacing: 1px;
      margin: 15px;
      transition: all 0.3s ease-in-out;

      &:hover {
        transform: translateY(-5px);
      }

      &--active {
        border-bottom: 1px solid #fff;
        text-transform: uppercase;
      }
    }
  }
}

.el-main {
  display: flex;
  justify-content: center;
  overflow: initial;
  padding: 50px;
}

.el-tiptap-editor {
  margin-bottom: 30px;

  &__wrapper {
    width: 60vw;
  }
}

.el-footer {
  align-items: center;
  display: flex;
  font-weight: 700;
  font-size: 14px;
  justify-content: center;
}

.github-corner:hover {
  .octo-arm {
    animation: octocat-wave 560ms ease-in-out;
  }
}

@media (max-width: 500px) {
  .github-corner:hover {
    .octo-arm {
      animation: none;
    }
  }

  .github-corner {
    .octo-arm {
      animation: octocat-wave 560ms ease-in-out;
    }
  }
}

@keyframes octocat-wave {
  0%,
  100% {
    transform: rotate(0);
  }

  20%,
  60% {
    transform: rotate(-25deg);
  }

  40%,
  80% {
    transform: rotate(10deg);
  }
}
</style>
