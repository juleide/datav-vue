<template>
  <div>
    <div class="datav-hearder" :style="{ background: isFixed ? '#171b22' : '' }">
      <div class="logo"></div>
      <div class="user">
        <div class="header-item">
          <n-dropdown
            :options="profileOpts"
            placement="bottom-end"
            :show-arrow="true"
            @select="handleProfileSelect"
          >
            <span class="user-link-wrap">
              <n-avatar
                round
                :size="20"
                :src="avatar + '?imageView2/1/w/80/h/80'"
              />
              <span class="user-link">
                {{ userName }}
              </span>
              <n-icon :size="14">
                <IconArrowDown />
              </n-icon>
            </span>
          </n-dropdown>
        </div>
      </div>
    </div>
    <div class="datav-nav">
      <div class="header-img">
        <img class="nav-img-text" :src="`${cdn}/datav/nav-img-text.png`">
        <div class="nav-img"></div>
      </div>
    </div>
  </div>
</template>

<script lang='ts'>
import { h, defineComponent, computed } from 'vue'
import { NIcon } from 'naive-ui'
import { useRouter } from 'vue-router'
import { useUserStore } from '@/store/user'
import { IconLogout, IconArrowDown, IconDocument } from '@/icons'

const cdn = import.meta.env.VITE_APP_CDN

export default defineComponent({
  name: 'NavHeader',
  components: {
    IconArrowDown,
  },
  props: {
    isFixed: {
      type: Boolean,
      default: false,
    },
  },
  setup() {
    const userStore = useUserStore()
    const router = useRouter()

    const profileOpts = [
      {
        label: '帮助文档',
        key: 'doc',
        icon: () => h(NIcon, null, { default: () => h(IconDocument) }),
      },
      {
        type: 'divider',
        key: 'd1',
      },
      {
        label: '退出',
        key: 'logout',
        icon: () => h(NIcon, null, { default: () => h(IconLogout) }),
      },
    ]

    const logout = async () => {
      await userStore.logout()
      router.push({ name: 'Login' })
    }

    const handleProfileSelect = (key: string) => {
      if (key === 'logout') {
        logout()
      }
    }

    return {
      cdn,
      userName: computed(() => userStore.name),
      avatar: computed(() => userStore.avatar),
      profileOpts,
      handleProfileSelect,
    }
  },
})
</script>

<style lang="scss">
@import '@/styles/mixins/function';

.datav-hearder {
  display: flex;
  position: fixed;
  top: 0;
  justify-content: space-between;
  z-index: 999;
  width: 100%;
  height: 30px;
  padding: 0 10px;
  background-image:
    linear-gradient(
      0deg,
      rgb(0 0 0 / 0%) 19%,
      #171717 100%
    );

  .logo {
    display: flex;
    flex: 1;
    align-items: center;
    padding-left: 18px;
  }

  .user {
    display: flex;
    justify-content: flex-end;
    padding-right: 15px;
    align-items: center;
    font-size: 14px;
    z-index: 9;
    min-width: 540px;

    .header-item {
      margin: 0 5px;
      padding: 0 10px;
      cursor: pointer;
      user-select: none;
      color: var(--datav-font-color);
      line-height: 20px;
      height: 20px;
    }

    .user-link-wrap {
      display: flex;
      align-items: center;
    }

    .user-link {
      display: inline-block;
      vertical-align: middle;
      margin: 0 4px;
      line-height: 20px;
      height: 20px;
      color: var(--datav-font-color);
    }
  }
}

.datav-nav {
  display: flex;
  position: absolute;
  top: 0;
  flex-direction: column;
  background: var(--datav-body-bg);
  width: 100%;
  height: 290px;

  .header-img {
    opacity: 1;
    z-index: 1;
  }

  .nav-img-text {
    z-index: 1;
    transform-origin: 0 0;
    transform: scale(0.5);
    top: 80px;
    left: 40px;
    position: absolute;
    user-select: none;
    cursor: pointer;
  }

  .nav-img {
    position: absolute;
    width: 100%;
    height: 290px;
    background-size: cover;
    background-position: center;
    background-image: url(com-cdn('datav/nav-img.png'));
  }
}
</style>
