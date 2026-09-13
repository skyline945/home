<template>
  <!-- 社交链接 -->
  <div class="social">
    <div class="link">
      <component
        v-for="item in socialLinks"
        :key="item.name"
        :is="item.url ? 'a' : 'span'"
        :href="item.url || undefined"
        :target="item.url ? '_blank' : undefined"
        :rel="item.url ? 'noopener' : undefined"
        class="social-item"
        :class="{ 'is-copy': !!item.copy }"
        @click="clickItem(item)"
        @mouseenter="socialTip = item.tip"
        @mouseleave="socialTip = '通过这里联系我吧'"
      >
        <img class="icon" :src="item.icon" height="24" />
      </component>
    </div>
    <span class="tip">{{ socialTip }}</span>
  </div>
</template>

<script setup>
import socialLinks from "@/assets/socialLinks.json";

// 社交链接提示
const socialTip = ref("通过这里联系我吧");

// 点击复制（用于没有链接、只有 ID 的项）
const clickItem = async (item) => {
  if (!item.copy) return;
  try {
    await navigator.clipboard.writeText(item.copy);
    ElMessage({
      message: `已复制：${item.copy}`,
      type: "success",
      grouping: true,
    });
  } catch {
    ElMessage({
      message: "复制失败，请手动复制",
      type: "error",
      grouping: true,
    });
  }
};
</script>

<style lang="scss" scoped>
.social {
  margin-top: 1rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  max-width: 460px;
  width: 100%;
  background-color: transparent;
  border-radius: 6px;
  backdrop-filter: blur(0);
  animation: fade 0.5s;
  transition:
    background-color 0.3s,
    backdrop-filter 0.3s;
  @media (max-width: 840px) {
    max-width: 100%;
    justify-content: center;
    .link {
      justify-content: space-evenly !important;
      width: 90%;
    }
    .tip {
      display: none !important;
    }
  }

  .link {
    display: flex;
    align-items: center;
    justify-content: center;
    .social-item {
      display: inherit;
      &.is-copy {
        cursor: pointer;
      }
      .icon {
        margin: 0 12px;
        transition: transform 0.3s;
        &:hover {
          transform: scale(1.1);
        }
        &:active {
          transform: scale(1);
        }
      }
    }
  }
  .tip {
    display: none;
    margin-top: 8px;
    font-size: 0.9rem;
    text-align: center;
    white-space: nowrap;
    animation: fade 0.5s;
  }
  @media (min-width: 768px) {
    &:hover {
      background-color: #00000040;
      backdrop-filter: blur(5px);
      .tip {
        display: block;
      }
    }
  }
}
</style>
