<template>
  <el-dialog
    v-model="visible"
    class="about-me-dialog"
    width="720px"
    align-center
    append-to-body
  >
    <div class="resume">
      <!-- 头部：昵称 + 座右铭 -->
      <header class="header">
        <h1 class="name">{{ resume.name }}</h1>
        <p class="tagline">{{ resume.tagline }}</p>
      </header>

      <!-- 联系方式 -->
      <section class="block">
        <h2 class="block-title">联系方式</h2>
        <div class="contact">
          <a
            v-for="c in resume.contact"
            :key="c.label"
            class="contact-item"
            :href="c.url || undefined"
            :target="c.url ? '_blank' : undefined"
            rel="noopener"
          >
            <span class="c-label">{{ c.label }}</span>
            <span class="c-value">{{ c.value }}</span>
          </a>
        </div>
      </section>

      <!-- 其余板块：教育 / 技能 / 项目 -->
      <section v-for="s in resume.sections" :key="s.title" class="block">
        <h2 class="block-title">{{ s.title }}</h2>

        <!-- 标签型（技能等） -->
        <div v-if="s.tags" class="tags">
          <span v-for="t in s.tags" :key="t" class="tag">{{ t }}</span>
        </div>

        <!-- 列表型（教育 / 项目等） -->
        <div v-else class="items">
          <div v-for="it in s.items" :key="it.title" class="item">
            <div class="item-top">
              <span class="item-title">{{ it.title }}</span>
              <span v-if="it.time" class="item-time">{{ it.time }}</span>
            </div>
            <div v-if="it.subtitle" class="item-subtitle">{{ it.subtitle }}</div>
            <p v-if="it.desc" class="item-desc">{{ it.desc }}</p>
          </div>
        </div>
      </section>
    </div>
  </el-dialog>
</template>

<script setup>
const props = defineProps({
  modelValue: { type: Boolean, default: false },
});
const emit = defineEmits(["update:modelValue"]);

// 弹窗显隐（双向绑定）
const visible = computed({
  get: () => props.modelValue,
  set: (v) => emit("update:modelValue", v),
});

// ===== 简历内容：在这里直接编辑即可 =====
const resume = reactive({
  name: "skyline",
  tagline: "人无法同时拥有青春和对青春的感受",

  // 联系方式
  contact: [
    { label: "邮箱", value: "2400013175@stu.pku.edu.cn", url: "mailto:2400013175@stu.pku.edu.cn" },
    { label: "GitHub", value: "github.com/skyline945", url: "https://github.com/skyline945" },
    { label: "B站", value: "space.bilibili.com/1455945055", url: "https://space.bilibili.com/1455945055" },
    { label: "QQ", value: "1296180033", url: "" },
  ],

  // 板块：有 tags 的渲染成标签，否则渲染成列表
  sections: [
    {
      title: "教育经历",
      items: [
        {
          title: "北京大学", // 根据邮箱 pku.edu.cn 猜的，请确认
          subtitle: "专业 / 年级（待补充）",
          time: "2024 - 至今",
          desc: "",
        },
      ],
    },
    {
      title: "技能",
      tags: ["待补充"],
    },
    {
      title: "项目经历",
      items: [
        {
          title: "待补充",
          subtitle: "",
          time: "",
          desc: "",
        },
      ],
    },
  ],
});
</script>

<style lang="scss">
// 弹窗整体（暗色毛玻璃，贴合站点风格）
.about-me-dialog {
  background: rgb(28 28 34 / 0.92);
  backdrop-filter: blur(24px);
  -webkit-backdrop-filter: blur(24px);
  border: 1px solid rgb(255 255 255 / 0.08);
  border-radius: 16px;
  max-width: 94vw;
  overflow: hidden;

  .el-dialog__header {
    padding: 0;
    .el-dialog__headerbtn {
      top: 18px;
      right: 18px;
      width: 34px;
      height: 34px;
      .el-dialog__close {
        color: #ffffff90;
        font-size: 20px;
        transition: color 0.2s, transform 0.2s;
        &:hover {
          color: #fff;
          transform: scale(1.1);
        }
      }
    }
  }

  .el-dialog__body {
    padding: 36px 40px;
    max-height: 72vh;
    overflow-y: auto;
    color: #fff;
    word-break: break-word;
  }
}

.resume {
  .header {
    margin-bottom: 28px;

    .name {
      margin: 0;
      font-family: "Pacifico-Regular";
      font-size: 2.4rem;
      font-weight: 400;
      color: #fff;
    }

    .tagline {
      margin: 8px 0 0;
      font-size: 1rem;
      color: #ffffff80;
    }
  }

  .block {
    margin-bottom: 26px;

    &:last-child {
      margin-bottom: 0;
    }

    .block-title {
      margin: 0 0 14px;
      font-size: 1.05rem;
      font-weight: 600;
      color: #ffffff;
      letter-spacing: 1px;

      &::before {
        content: "";
        display: inline-block;
        width: 4px;
        height: 16px;
        margin-right: 8px;
        vertical-align: -2px;
        border-radius: 2px;
        background: rgb(244 167 89);
      }
    }
  }

  .contact {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;

    .contact-item {
      display: inline-flex;
      align-items: center;
      gap: 8px;
      padding: 8px 14px;
      border-radius: 8px;
      background: rgb(255 255 255 / 0.06);
      color: #ffffffd9;
      font-size: 0.9rem;
      text-decoration: none;
      transition: background 0.2s;

      &:hover {
        background: rgb(255 255 255 / 0.14);
      }

      .c-label {
        color: #ffffff70;
      }
    }
  }

  .tags {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;

    .tag {
      padding: 6px 14px;
      border-radius: 999px;
      background: rgb(244 167 89 / 0.18);
      border: 1px solid rgb(244 167 89 / 0.35);
      color: #f4a759;
      font-size: 0.9rem;
    }
  }

  .items {
    .item {
      margin-bottom: 18px;

      &:last-child {
        margin-bottom: 0;
      }

      .item-top {
        display: flex;
        align-items: baseline;
        justify-content: space-between;
        gap: 12px;

        .item-title {
          font-size: 1.05rem;
          font-weight: 600;
          color: #fff;
        }

        .item-time {
          flex-shrink: 0;
          font-size: 0.85rem;
          color: #ffffff70;
        }
      }

      .item-subtitle {
        margin-top: 4px;
        font-size: 0.9rem;
        color: #ffffff90;
      }

      .item-desc {
        margin: 8px 0 0;
        font-size: 0.9rem;
        line-height: 1.7;
        color: #ffffffb0;
      }
    }
  }
}

@media (max-width: 480px) {
  .about-me-dialog .el-dialog__body {
    padding: 28px 22px;
  }
}
</style>
