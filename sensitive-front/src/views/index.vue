<template>
  <div class="app-container home">
    <div class="home-marble home-marble--one"></div>
    <div class="home-marble home-marble--two"></div>

    <section class="welcome-card">
      <div class="welcome-main">
        <div class="brand-line">
          <span class="brand-badge"><img :src="logo" alt="" /></span>
          <span>{{ title }}</span>
        </div>
        <h2>{{ greeting }}，{{ userName }}</h2>
        <p>{{ todayText }} · 欢迎使用 {{ title }}</p>

        <div class="status-strip">
          <div v-for="item in highlights" :key="item.label" class="status-item">
            <b>{{ item.value }}</b>
            <span>{{ item.label }}</span>
          </div>
        </div>
      </div>

      <div class="quick-panel">
        <div class="quick-heading">
          <span>常用入口</span>
          <el-icon><Sunny /></el-icon>
        </div>
        <button v-for="item in quickLinks" :key="item.path" class="quick-action" type="button" @click="go(item.path)">
          <span class="quick-icon" :style="{ background: item.bg, color: item.color }">
            <el-icon :size="22"><component :is="item.icon" /></el-icon>
          </span>
          <span class="quick-meta">
            <span class="quick-name">{{ item.name }}</span>
            <span class="quick-desc">{{ item.desc }}</span>
          </span>
          <span class="quick-arrow"><el-icon><ArrowRight /></el-icon></span>
        </button>
      </div>
    </section>

    <section class="dashboard-grid">
      <article class="glass-card stack-card">
        <header class="card-head">
          <span>技术栈</span>
          <small>Stack</small>
        </header>
        <div class="stack-body">
          <div v-for="g in stacks" :key="g.label" class="stack-group">
            <span class="stack-label">{{ g.label }}</span>
            <div class="stack-tags">
              <span v-for="t in g.items" :key="t" class="stack-tag">{{ t }}</span>
            </div>
          </div>
        </div>
      </article>

      <article class="glass-card info-card">
        <header class="card-head">
          <span>项目信息</span>
          <small>Project</small>
        </header>
        <ul class="info-list">
          <li><span>系统名称</span><b>{{ title }}</b></li>
          <li><span>框架基座</span><b>sensitive</b></li>
          <li><span>前端技术</span><b>Vue 3 + Element Plus</b></li>
          <li><span>后端技术</span><b>Spring Boot 3</b></li>
        </ul>
      </article>
    </section>
  </div>
</template>

<script setup name="Index">
import useUserStore from '@/store/modules/user'
import logo from '@/assets/logo/sensitive-logo.svg'

const router = useRouter()
const userStore = useUserStore()

const title = import.meta.env.VITE_APP_TITLE
const userName = computed(() => userStore.name || '管理员')

const greeting = computed(() => {
  const h = new Date().getHours()
  if (h < 6) return '凌晨好'
  if (h < 11) return '早上好'
  if (h < 13) return '中午好'
  if (h < 18) return '下午好'
  return '晚上好'
})

const todayText = computed(() => {
  const d = new Date()
  const week = ['日', '一', '二', '三', '四', '五', '六'][d.getDay()]
  return `${d.getFullYear()}年${d.getMonth() + 1}月${d.getDate()}日 星期${week}`
})

const quickLinks = [
  { name: '用户管理', desc: '账号与组织', path: '/system/user', icon: 'UserFilled', color: '#3b82f6', bg: 'rgba(59,130,246,.12)' },
  { name: '角色管理', desc: '权限分配', path: '/system/role', icon: 'Avatar', color: '#8b5cf6', bg: 'rgba(139,92,246,.12)' },
  { name: '个人中心', desc: '我的资料', path: '/user/profile', icon: 'Setting', color: '#64748b', bg: 'rgba(100,116,139,.12)' }
]

const highlights = [
  { value: '3', label: '常用入口' },
  { value: 'Vue 3', label: '前端基座' },
  { value: 'Boot 3', label: '后端基座' }
]

const stacks = [
  { label: '后端', items: ['Spring Boot 3', 'Spring Security', 'JWT', 'MyBatis', 'Druid', 'Redis', 'Fastjson'] },
  { label: '前端', items: ['Vue 3', 'Vue Router', 'Pinia', 'Element Plus', 'Axios', 'Sass', 'ECharts', 'Vite'] }
]

function go(path) {
  router.push(path).catch(() => {})
}
</script>

<style scoped lang="scss">
.home {
  position: relative;
  min-height: calc(100vh - 86px);
  padding: 28px !important;
  overflow: hidden;
  background:
    linear-gradient(118deg, transparent 0 14%, rgba(96, 130, 170, 0.14) 14.2%, transparent 15.2% 100%),
    linear-gradient(142deg, transparent 0 32%, rgba(45, 112, 192, 0.12) 32.2%, rgba(255, 255, 255, 0.5) 32.7%, transparent 34.1% 100%),
    linear-gradient(154deg, transparent 0 49%, rgba(128, 158, 195, 0.16) 49.2%, transparent 50.2% 100%),
    radial-gradient(ellipse at 18% 10%, rgba(255, 255, 255, 0.92), transparent 36%),
    radial-gradient(ellipse at 72% 6%, rgba(183, 216, 251, 0.62), transparent 38%),
    repeating-radial-gradient(circle at 22% 34%, rgba(78, 105, 140, 0.045) 0 1px, transparent 1px 9px),
    linear-gradient(135deg, #fbfdff 0%, #eef7ff 46%, #ffffff 100%);
  box-shadow: none !important;
  --home-primary: #2f7df6;
  --home-primary-soft: #73b7ff;
  --home-ink: #1b2b44;
  --home-muted: #6e7e96;
  --home-border: rgba(91, 128, 174, 0.18);
  --home-line: rgba(71, 104, 148, 0.12);
  --home-shadow: 0 24px 64px rgba(47, 88, 140, 0.14), 0 8px 22px rgba(52, 92, 143, 0.08);
}

.home::before,
.home::after {
  position: absolute;
  inset: 0;
  content: "";
  pointer-events: none;
}

.home::before {
  opacity: 0.8;
  background:
    linear-gradient(126deg, transparent 0 18%, rgba(60, 91, 128, 0.13) 18.1%, rgba(255, 255, 255, 0.55) 18.45%, transparent 19.4% 100%),
    linear-gradient(148deg, transparent 0 55%, rgba(87, 126, 174, 0.13) 55.15%, rgba(255, 255, 255, 0.5) 55.5%, transparent 56.7% 100%),
    linear-gradient(24deg, transparent 0 65%, rgba(255, 255, 255, 0.72) 65.35%, rgba(96, 145, 202, 0.1) 65.9%, transparent 67% 100%);
}

.home::after {
  opacity: 0.45;
  mix-blend-mode: multiply;
  background:
    repeating-radial-gradient(circle at 72% 58%, rgba(255, 255, 255, 0.5) 0 1px, transparent 1px 11px),
    linear-gradient(114deg, transparent 0 38%, rgba(73, 109, 154, 0.1) 38.1%, transparent 38.9% 100%),
    radial-gradient(ellipse at 36% 68%, rgba(255, 255, 255, 0.38), transparent 40%);
}

.home-marble {
  position: absolute;
  border-radius: 50%;
  filter: blur(4px);
  opacity: 0.62;
  pointer-events: none;

  &--one {
    width: 360px;
    height: 360px;
    top: -160px;
    right: 10%;
    background: radial-gradient(circle, rgba(93, 159, 239, 0.18), transparent 70%);
  }

  &--two {
    width: 320px;
    height: 320px;
    bottom: -140px;
    left: 5%;
    background: radial-gradient(circle, rgba(255, 255, 255, 0.78), transparent 70%);
  }
}

.welcome-card,
.dashboard-grid {
  position: relative;
  z-index: 1;
}

.welcome-card {
  display: flex;
  align-items: stretch;
  justify-content: space-between;
  gap: 26px;
  min-height: 260px;
  padding: 30px;
  margin-bottom: 20px;
  overflow: hidden;
  border: 1px solid rgba(255, 255, 255, 0.86);
  border-radius: 18px;
  background: rgba(255, 255, 255, 0.76);
  backdrop-filter: blur(24px);
  box-shadow: var(--home-shadow);

  &::before {
    position: absolute;
    inset: 0;
    content: "";
    pointer-events: none;
    background:
      linear-gradient(135deg, rgba(255, 255, 255, 0.95), transparent 38%),
      linear-gradient(152deg, transparent 0 58%, rgba(91, 151, 225, 0.09) 59%, transparent 68%);
  }

  > * {
    position: relative;
    z-index: 1;
  }
}

.welcome-main {
  display: flex;
  flex: 1;
  min-width: 0;
  flex-direction: column;
  justify-content: center;

  .brand-line {
    display: inline-flex;
    align-items: center;
    gap: 12px;
    width: fit-content;
    margin-bottom: 24px;
    color: rgba(47, 125, 246, 0.9);
    font-size: 14px;
    font-weight: 700;
  }

  .brand-badge {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 50px;
    height: 50px;
    border-radius: 16px;
    background: linear-gradient(135deg, #2f7df6, #7fc5ff);
    box-shadow: 0 12px 26px rgba(47, 125, 246, 0.24);

    img {
      display: block;
      width: 32px;
      height: 32px;
      filter: drop-shadow(0 6px 10px rgba(24, 91, 166, 0.18));
    }
  }

  h2 {
    margin: 0;
    color: var(--home-ink);
    font-size: clamp(28px, 3vw, 42px);
    line-height: 1.16;
    font-weight: 700;
    letter-spacing: 0;
  }

  p {
    margin: 14px 0 0;
    color: var(--home-muted);
    font-size: 15px;
  }
}

.status-strip {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 150px));
  gap: 12px;
  max-width: 520px;
  margin-top: 34px;
}

.status-item {
  min-width: 0;
  padding: 14px 16px;
  border: 1px solid rgba(91, 128, 174, 0.14);
  border-radius: 14px;
  background: rgba(255, 255, 255, 0.56);

  b,
  span {
    display: block;
  }

  b {
    color: var(--home-ink);
    font-size: 18px;
    line-height: 1.2;
  }

  span {
    margin-top: 6px;
    color: var(--home-muted);
    font-size: 12px;
  }
}

.quick-panel {
  flex: 0 0 360px;
  padding: 4px 0;
}

.quick-heading {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 14px;
  color: var(--home-ink);
  font-size: 15px;
  font-weight: 700;

  .el-icon {
    color: rgba(47, 125, 246, 0.44);
    font-size: 30px;
  }
}

.quick-action {
  display: flex;
  align-items: center;
  gap: 13px;
  width: 100%;
  min-height: 66px;
  padding: 11px 12px;
  border: 1px solid rgba(91, 128, 174, 0.14);
  border-radius: 12px;
  background: rgba(255, 255, 255, 0.74);
  color: inherit;
  cursor: pointer;
  text-align: left;
  transition: transform 0.2s ease, box-shadow 0.2s ease, border-color 0.2s ease, background 0.2s ease;

  & + .quick-action {
    margin-top: 10px;
  }

  &:hover {
    transform: translateY(-2px);
    border-color: rgba(47, 125, 246, 0.28);
    background: rgba(255, 255, 255, 0.92);
    box-shadow: 0 12px 24px rgba(47, 88, 140, 0.12);
  }

  .quick-icon {
    flex-shrink: 0;
    width: 44px;
    height: 44px;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 12px;
  }

  .quick-meta {
    display: flex;
    flex: 1;
    min-width: 0;
    flex-direction: column;
  }

  .quick-name {
    color: var(--home-ink);
    font-size: 15px;
    font-weight: 700;
  }

  .quick-desc {
    margin-top: 4px;
    color: var(--home-muted);
    font-size: 12px;
  }

  .quick-arrow {
    display: flex;
    align-items: center;
    justify-content: center;
    color: rgba(47, 125, 246, 0.52);
    font-size: 18px;
  }
}

.dashboard-grid {
  display: grid;
  grid-template-columns: minmax(0, 1fr) minmax(320px, 0.45fr);
  gap: 20px;
}

.glass-card {
  min-width: 0;
  overflow: hidden;
  border: 1px solid rgba(255, 255, 255, 0.82);
  border-radius: 16px;
  background: rgba(255, 255, 255, 0.72);
  backdrop-filter: blur(18px);
  box-shadow: 0 16px 42px rgba(47, 88, 140, 0.1), 0 4px 14px rgba(52, 92, 143, 0.06);
}

.card-head {
  display: flex;
  align-items: baseline;
  justify-content: space-between;
  padding: 18px 20px;
  border-bottom: 1px solid var(--home-line);
  color: var(--home-ink);
  font-weight: 600;

  small {
    color: rgba(110, 126, 150, 0.58);
    font-size: 12px;
    font-weight: 600;
  }
}

.stack-body {
  padding: 18px 24px 20px;
}

.stack-group {
  display: flex;
  align-items: flex-start;
  padding: 12px 0;

  & + .stack-group {
    border-top: 1px dashed var(--home-line);
  }

  .stack-label {
    flex-shrink: 0;
    width: 54px;
    color: var(--home-muted);
    font-size: 14px;
    font-weight: 600;
    line-height: 30px;
  }

  .stack-tags {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
  }

  .stack-tag {
    display: inline-flex;
    align-items: center;
    min-height: 30px;
    padding: 0 14px;
    border: 1px solid rgba(91, 128, 174, 0.18);
    border-radius: 999px;
    background: rgba(255, 255, 255, 0.56);
    color: var(--home-muted);
    font-size: 13px;
  }
}

.info-list {
  margin: 0;
  padding: 18px 24px 22px;
  list-style: none;

  li {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 24px;
    padding: 13px 4px;
    font-size: 14px;

    & + li {
      border-top: 1px dashed var(--home-line);
    }

    span {
      color: var(--home-muted);
    }

    b {
      color: var(--home-ink);
      font-weight: 600;
      text-align: right;
    }
  }
}

html.dark .home {
  --home-ink: #eef5ff;
  --home-muted: #a7b5c8;
  --home-border: rgba(255, 255, 255, 0.12);
  --home-line: rgba(255, 255, 255, 0.08);
  background:
    linear-gradient(118deg, rgba(128, 187, 255, 0.08) 0 1px, transparent 1px 100%),
    radial-gradient(circle at 68% 18%, rgba(45, 95, 153, 0.3), transparent 32%),
    linear-gradient(135deg, #0f1726 0%, #162132 48%, #0f1726 100%);

  .welcome-card,
  .glass-card {
    border-color: rgba(255, 255, 255, 0.1);
    background: rgba(20, 29, 43, 0.82);
    box-shadow: 0 24px 60px rgba(0, 0, 0, 0.34);
  }

  .status-item,
  .quick-action,
  .stack-tag {
    border-color: rgba(255, 255, 255, 0.1);
    background: rgba(255, 255, 255, 0.05);
  }

  .quick-action:hover {
    background: rgba(255, 255, 255, 0.08);
  }
}

@media (max-width: 1180px) {
  .welcome-card {
    flex-direction: column;
  }

  .quick-panel {
    flex-basis: auto;
    display: grid;
    grid-template-columns: repeat(3, minmax(0, 1fr));
    gap: 10px;
  }

  .quick-heading {
    grid-column: 1 / -1;
    margin-bottom: 2px;
  }

  .quick-action + .quick-action {
    margin-top: 0;
  }

  .dashboard-grid {
    grid-template-columns: 1fr;
  }
}

@media (max-width: 768px) {
  .home {
    min-height: calc(100vh - 56px);
    padding: 16px !important;
  }

  .welcome-card {
    padding: 22px;
  }

  .status-strip,
  .quick-panel {
    grid-template-columns: 1fr;
  }

  .welcome-main .brand-line {
    margin-bottom: 18px;
  }

  .stack-group,
  .info-list li {
    flex-direction: column;
    align-items: flex-start;
    gap: 8px;
  }

  .info-list li b {
    text-align: left;
  }
}
</style>
