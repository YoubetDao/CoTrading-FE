# CoTrading-FE

## 项目概述与目标

Your AI Copilot for Smarter Crypto Trading

一句话说明：Cotrading 通过理解你的交易偏好并融合 CEX、DEX 与链上数据，以多智能体协作在原生交易工作流中提供实时、可执行、可解释的策略与风控建议。

- 同时理解你与市场：持续学习你的交易风格，跨 CEX/DEX/链上整合数据，将人的直觉与机器精度结合。
- 多智能体协作：由链上、情绪、量化、技术、风险等子智能体（SubAgents）协同运作，输出全景洞见与实时策略。
- 一体化交易体验：AI 嵌入交易工作流，结合交互式 K 线、仪表盘与分析画布，零上下文切换、低摩擦。

目标
- 个性化投研与策略共创，提升决策速度与质量
- 多源数据融合与可解释洞见，强化市场把握
- 策略实时协同与自动化执行接口，缩短从想法到落地
- 风险识别/预警与仓位建议，守住风控边界
- 可视化与可追溯记录，沉淀方法论

## 技术栈与依赖说明
- 框架与语言：Next.js 15（App Router）、React 19 RC、TypeScript
- UI/状态：Tailwind CSS、Radix UI、TanStack Query
- 数据与鉴权：Drizzle ORM + Postgres、NextAuth、Redis（可选）
- Web3：wagmi、viem、RainbowKit
- 工具与质量：ESLint、Biome、Playwright E2E、@vercel/*（Analytics/Postgres/Blob 等）

## 部署与使用指南
环境要求：Node 18+、pnpm 9.x

本地开发
- 安装依赖：`pnpm i`
- 配置环境变量：创建 `.env.local`（示例见下）
- 初始化数据库：`pnpm db:migrate`
- 启动开发：`pnpm dev`（默认 http://localhost:3000）
- 测试与质量：`pnpm test`、`pnpm lint`、`pnpm format`

构建与部署
- 构建：`pnpm build`（先运行 DB 迁移再构建）
- 生产启动：`pnpm start`
- 部署建议：Vercel；或自托管 Node 环境，确保数据库与环境变量可用

数据库与工具（可选）
- 生成/迁移/可视化：`pnpm db:generate`、`pnpm db:migrate`、`pnpm db:studio`

## 环境变量示例
- `NEXT_PUBLIC_BACKEND_URL`（可选；不设时本地默认 `http://localhost:8000`）
- `DATABASE_URL`（Postgres 连接串）
- `NEXTAUTH_URL`、`NEXTAUTH_SECRET`
- `REDIS_URL`（可选）

## OpenAPI 客户端（可选）
- 生成：`pnpm openapi:client`（默认 predev/prebuild 跳过；在仓库根也可执行 `make -C .. codegen-frontend-client`）
