# App Showcase Portfolio - 技术规格文档

---

## 1. 组件清单

### shadcn/ui 组件

| 组件 | 用途 | 自定义需求 |
|------|------|-----------|
| Button | 各类按钮 | 渐变背景、发光效果 |
| Card | 应用卡片、信息卡片 | 玻璃拟态样式 |
| Dialog | 登录模态框、详情弹窗 | 玻璃背景、动画 |
| Input | 搜索框、表单输入 | 深色主题、发光聚焦 |
| Tabs | 用户面板标签页 | 下划线指示器动画 |
| Badge | 技术栈标签、分类标签 | 渐变背景 |
| Avatar | 用户头像 | 发光边框 |
| DropdownMenu | 导航下拉、筛选菜单 | 玻璃样式 |
| Tooltip | 提示信息 | 深色主题 |
| Skeleton | 加载骨架屏 | 脉冲动画 |
| ScrollArea | 滚动区域 | 自定义滚动条 |
| Separator | 分隔线 | 渐变颜色 |

### 自定义组件

| 组件 | 用途 | 复杂度 |
|------|------|--------|
| GlassCard | 玻璃拟态卡片容器 | 中 |
| GradientText | 渐变文字效果 | 低 |
| FloatingShapes | 3D 浮动几何形状 | 高 |
| AnimatedGrid | 应用卡片网格（含筛选动画） | 高 |
| TiltCard | 3D 倾斜悬停卡片 | 高 |
| AppCarousel | 应用截图轮播 | 中 |
| AuthModal | 登录/注册模态框 | 中 |
| SearchInput | 带图标的搜索框 | 低 |
| ViewToggle | 网格/列表视图切换 | 低 |
| StatsCounter | 数字统计动画 | 中 |
| Timeline | 开发历程时间轴 | 中 |
| GlowButton | 发光渐变按钮 | 低 |

---

## 2. 动画实现规划

### 动画库选择

| 库 | 用途 | 理由 |
|----|------|------|
| Framer Motion | 主要动画库 | React 原生支持、声明式 API、手势支持 |
| CSS Animations | 简单循环动画 | 性能最优、背景渐变、浮动效果 |
| GSAP (可选) | 复杂时间轴 | 如需精确控制时考虑 |

### 动画实现表

| 动画 | 库 | 实现方案 | 复杂度 |
|------|-----|---------|--------|
| 页面加载序列 | Framer Motion | AnimatePresence + staggerChildren | 高 |
| 背景渐变流动 | CSS Animation | @keyframes 渐变位置变化 | 中 |
| 3D 浮动形状 | CSS Animation + Framer | transform: rotate/translate, 循环动画 | 高 |
| 滚动触发显示 | Framer Motion | whileInView + viewport | 中 |
| 卡片 3D 倾斜 | Framer Motion | useMotionValue + useTransform | 高 |
| 筛选布局动画 | Framer Motion | layout + AnimatePresence | 高 |
| 模态框开关 | Framer Motion | AnimatePresence, scale + opacity | 中 |
| 按钮悬停发光 | CSS + Framer | whileHover, box-shadow 过渡 | 低 |
| 数字计数动画 | Framer Motion | useSpring + useInView | 中 |
| 轮播滑动 | Framer Motion | drag + animate | 中 |
| 时间轴绘制 | Framer Motion | whileInView + pathLength | 中 |
| 标签页切换 | Framer Motion | layoutId 共享元素 | 中 |

### 动画组件封装

```typescript
// FadeInView - 滚动触发淡入
interface FadeInViewProps {
  children: React.ReactNode;
  delay?: number;
  direction?: 'up' | 'down' | 'left' | 'right';
}

// TiltCard - 3D 倾斜卡片
interface TiltCardProps {
  children: React.ReactNode;
  maxTilt?: number;
  scale?: number;
}

// AnimatedCounter - 数字动画
interface AnimatedCounterProps {
  value: number;
  duration?: number;
  suffix?: string;
}

// StaggerContainer - 错开动画容器
interface StaggerContainerProps {
  children: React.ReactNode;
  staggerDelay?: number;
}
```

---

## 3. 项目文件结构

```
app/
├── app/                          # Next.js App Router
│   ├── page.tsx                  # 主页面 (Hero + Gallery)
│   ├── layout.tsx                # 根布局
│   ├── globals.css               # 全局样式
│   ├── apps/
│   │   └── [slug]/
│   │       └── page.tsx          # 应用详情页
│   ├── about/
│   │   └── page.tsx              # 关于页面
│   └── api/
│       └── auth/
│           └── [...nextauth]/
│               └── route.ts      # NextAuth 配置
├── components/
│   ├── ui/                       # shadcn/ui 组件
│   │   ├── button.tsx
│   │   ├── card.tsx
│   │   ├── dialog.tsx
│   │   ├── input.tsx
│   │   ├── tabs.tsx
│   │   ├── badge.tsx
│   │   ├── avatar.tsx
│   │   ├── dropdown-menu.tsx
│   │   ├── tooltip.tsx
│   │   ├── skeleton.tsx
│   │   ├── scroll-area.tsx
│   │   └── separator.tsx
│   ├── layout/                   # 布局组件
│   │   ├── Navbar.tsx            # 导航栏
│   │   ├── Footer.tsx            # 页脚
│   │   ├── MobileNav.tsx         # 移动端导航
│   │   └── Container.tsx         # 内容容器
│   ├── sections/                 # 页面区块
│   │   ├── Hero.tsx              # Hero 区域
│   │   ├── AppGallery.tsx        # 应用画廊
│   │   ├── AppDetail.tsx         # 应用详情
│   │   ├── AboutSection.tsx      # 关于部分
│   │   └── ContactSection.tsx    # 联系表单
│   ├── effects/                  # 特效组件
│   │   ├── FloatingShapes.tsx    # 3D 浮动形状
│   │   ├── GradientBackground.tsx # 渐变背景
│   │   ├── GlowEffect.tsx        # 发光效果
│   │   └── ParticleField.tsx     # 粒子场 (可选)
│   ├── cards/                    # 卡片组件
│   │   ├── AppCard.tsx           # 应用卡片
│   │   ├── GlassCard.tsx         # 玻璃卡片
│   │   └── TiltCard.tsx          # 3D 倾斜卡片
│   ├── animations/               # 动画组件
│   │   ├── FadeInView.tsx        # 滚动淡入
│   │   ├── StaggerContainer.tsx  # 错开动画
│   │   ├── AnimatedCounter.tsx   # 数字动画
│   │   └── AnimatedText.tsx      # 文字动画
│   ├── forms/                    # 表单组件
│   │   ├── SearchInput.tsx       # 搜索框
│   │   ├── ContactForm.tsx       # 联系表单
│   │   └── AuthForm.tsx          # 登录表单
│   └── modals/                   # 模态框
│       └── AuthModal.tsx         # 认证模态框
├── hooks/                        # 自定义 Hooks
│   ├── useMousePosition.ts       # 鼠标位置
│   ├── useScrollProgress.ts      # 滚动进度
│   ├── useLocalStorage.ts        # 本地存储
│   ├── useDebounce.ts            # 防抖
│   └── useMediaQuery.ts          # 媒体查询
├── lib/                          # 工具函数
│   ├── utils.ts                  # 通用工具
│   ├── animations.ts             # 动画配置
│   └── constants.ts              # 常量
├── types/                        # TypeScript 类型
│   └── index.ts                  # 类型定义
├── data/                         # 数据文件
│   └── apps.ts                   # 应用数据
├── public/                       # 静态资源
│   ├── images/
│   │   ├── apps/                 # 应用截图
│   │   ├── avatar/               # 头像
│   │   └── shapes/               # 3D 形状
│   └── fonts/
├── styles/
│   └── animations.css            # 动画样式
├── next.config.js
├── tailwind.config.ts
├── tsconfig.json
└── package.json
```

---

## 4. 依赖清单

### 核心依赖

```json
{
  "dependencies": {
    "next": "^14.0.0",
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "typescript": "^5.0.0"
  }
}
```

### UI 与样式

```json
{
  "dependencies": {
    "tailwindcss": "^3.4.0",
    "@tailwindcss/typography": "^0.5.0",
    "class-variance-authority": "^0.7.0",
    "clsx": "^2.0.0",
    "tailwind-merge": "^2.0.0"
  }
}
```

### 动画

```json
{
  "dependencies": {
    "framer-motion": "^11.0.0"
  }
}
```

### 认证

```json
{
  "dependencies": {
    "next-auth": "^4.24.0"
  }
}
```

### 图标

```json
{
  "dependencies": {
    "lucide-react": "^0.300.0"
  }
}
```

### 工具

```json
{
  "dependencies": {
    "@radix-ui/react-*": "latest",
    "zod": "^3.22.0"
  }
}
```

---

## 5. 关键实现细节

### 玻璃拟态效果

```css
.glass {
  background: rgba(30, 41, 59, 0.7);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  border: 1px solid rgba(255, 255, 255, 0.1);
}
```

### 渐变文字

```css
.gradient-text {
  background: linear-gradient(135deg, #8b5cf6, #06b6d4);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}
```

### 3D 卡片倾斜

```typescript
// 使用 Framer Motion
const x = useMotionValue(0);
const y = useMotionValue(0);

const rotateX = useTransform(y, [-100, 100], [8, -8]);
const rotateY = useTransform(x, [-100, 100], [-8, 8]);

<motion.div
  style={{ x, y, rotateX, rotateY, perspective: 1000 }}
  whileHover={{ scale: 1.02 }}
/>
```

### 布局动画 (筛选)

```typescript
// Framer Motion layout
<motion.div layout layoutId={app.id}>
  <AppCard app={app} />
</motion.div>
```

---

## 6. 性能优化

### 图片优化
- 使用 Next.js Image 组件
- WebP 格式
- 懒加载
- 响应式尺寸

### 动画优化
- 使用 transform 和 opacity
- will-change 提示
- 减少重绘
- prefers-reduced-motion 支持

### 代码优化
- 组件懒加载
- 动态导入
- Tree shaking

---

## 7. 开发计划

### 阶段 1: 基础搭建
1. 初始化项目
2. 配置 Tailwind 主题
3. 安装 shadcn/ui 组件
4. 设置全局样式

### 阶段 2: 核心组件
1. 布局组件 (Navbar, Footer)
2. 玻璃拟态卡片
3. 动画组件封装
4. 特效组件 (背景、浮动形状)

### 阶段 3: 页面开发
1. Hero 区域
2. 应用画廊
3. 应用详情页
4. 关于/联系页

### 阶段 4: 交互功能
1. 筛选与搜索
2. 视图切换
3. 认证模态框
4. 动画优化

### 阶段 5: 部署
1. 构建测试
2. 性能优化
3. Vercel 部署
