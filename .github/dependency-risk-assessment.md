# 依赖升级风险评估

> 评估日期：2026-02-17
> 评估人：developer
> 项目：starter-nextjs-wagmi
> 总计过时依赖：33 个

## 主分支状态

| 检查项              | 状态    | 备注                                               |
| ------------------- | ------- | -------------------------------------------------- |
| TypeScript 类型检查 | ✅ 通过 | `tsc --noEmit -p tsconfig.app.json`                |
| ESLint              | ✅ 通过 | `eslint .`                                         |
| 构建编译 (webpack)  | ✅ 通过 | 61s 编译成功                                       |
| SSG 静态生成        | ❌ 失败 | `localStorage.getItem` 在 SSR 中被调用（预存问题） |
| 构建 (turbopack)    | ❌ 失败 | webpack 配置未迁移到 turbopack（Next 16 默认）     |

## 已知安全漏洞

- **next@16.0.8** — 已废弃，存在安全漏洞，需升级到补丁版本（见 nextjs.org/blog/security-update-2025-12-11）

## peer 依赖冲突

- `@heroui/theme@2.4.23` 需要 `tailwindcss>=4.0.0`，当前为 `3.4.16`
- `@hairy/react-lib@1.46.0` 需要 `react@^18.2.0`，当前为 `19.2.1`
- `use-sync-external-store@1.2.0`（wagmi 传递依赖）需要 `react@^16-18`，当前为 `19.2.1`

---

## 🔴 高风险（Major 升级，需大幅改动）

| 包                            | 当前     | 最新   | 风险说明                                                                                                                                                         |
| ----------------------------- | -------- | ------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `next`                        | 16.0.8   | 16.1.6 | **安全补丁，必须升级**。16.0.8 已标记为废弃且有安全漏洞。升级到 16.1.x 应是 patch 级别变更，风险低。                                                             |
| `wagmi`                       | 2.16.9   | 3.4.4  | **Major 升级**。v3 改变了核心 API，需迁移连接器、hooks 等。涉及 `@rainbow-me/rainbowkit` 兼容性。**风险极高，建议单独分支处理。**                                |
| `tailwindcss`                 | 3.4.16   | 4.1.18 | **Major 升级**。v4 配置系统全面重构（CSS-first config），需迁移 `tailwind.config.js`、PostCSS 配置、所有工具类。同时解决 `@heroui/theme` peer 依赖。**风险高。** |
| `eslint`                      | 9.26.0   | 10.0.0 | **Major 升级**。可能影响所有 ESLint 插件兼容性。需与 `@antfu/eslint-config` 配合验证。                                                                           |
| `@antfu/eslint-config`        | 4.13.0   | 7.4.3  | **3 个 Major 跨版本**。配置 API 可能完全变化，需重写 `eslint.config.mjs`。                                                                                       |
| `@eslint-react/eslint-plugin` | 1.49.0   | 2.13.0 | **Major 升级**。规则名称/配置可能变更。                                                                                                                          |
| `eslint-plugin-react-hooks`   | 5.2.0    | 7.0.1  | **2 个 Major 跨版本**。需验证与 eslint 10 兼容性。                                                                                                               |
| `@hairy/lnv`                  | 6.3.0    | 9.1.0  | **3 个 Major 跨版本**。CLI 接口可能变化，影响所有 npm scripts（`lnv --`）。                                                                                      |
| `@types/node`                 | 22.15.16 | 25.2.3 | **3 个 Major 跨版本**。可能引入类型不兼容。                                                                                                                      |
| `unplugin-auto-import`        | 19.2.0   | 21.0.0 | **2 个 Major 跨版本**。插件 API/配置可能变更。                                                                                                                   |
| `vitest`                      | 3.1.3    | 4.0.18 | **Major 升级**。测试 API 可能变化。                                                                                                                              |
| `tailwind-variants`           | 0.3.0    | 3.2.2  | **3 个 Major 跨版本**（0.x → 3.x）。API 变化极大。                                                                                                               |

## 🟡 中风险（Minor/Patch 但跨度较大）

| 包                            | 当前    | 最新    | 风险说明                                                          |
| ----------------------------- | ------- | ------- | ----------------------------------------------------------------- |
| `framer-motion`               | 12.10.1 | 12.34.0 | Minor 更新跨度大（24 个 minor），但同一 major，风险可控。         |
| `viem`                        | 2.41.2  | 2.46.1  | Minor 更新，Web3 核心库，需验证合约交互。                         |
| `eslint-plugin-format`        | 1.0.1   | 1.4.0   | Minor 更新，风险低。                                              |
| `eslint-plugin-react-refresh` | 0.4.20  | 0.5.0   | Minor 更新（0.x 阶段），可能有 breaking changes。                 |
| `etherlib-generator`          | 0.2.1   | 0.3.3   | Minor 更新（0.x 阶段），影响代码生成。需验证 `postinstall` 脚本。 |
| `iconify-svgo-loader`         | 1.0.5   | 1.2.0   | Minor 更新，风险低。                                              |
| `react`                       | 19.2.1  | 19.2.4  | Patch 更新，风险极低。                                            |
| `react-dom`                   | 19.2.1  | 19.2.4  | Patch 更新，风险极低。                                            |

## 🟢 低风险（Patch/Minor 小幅更新）

| 包                      | 当前    | 最新    | 风险说明                   |
| ----------------------- | ------- | ------- | -------------------------- |
| `@heroui/react`         | 2.8.5   | 2.8.9   | Patch 更新。               |
| `@heroui/theme`         | 2.4.23  | 2.4.26  | Patch 更新。               |
| `@tanstack/react-query` | 5.90.12 | 5.90.21 | Patch 更新。               |
| `autoprefixer`          | 10.4.19 | 10.4.24 | Patch 更新。               |
| `@hairy/ether-lib`      | 1.46.0  | 1.47.0  | Minor 更新，自有库。       |
| `@hairy/react-lib`      | 1.46.0  | 1.47.0  | Minor 更新，自有库。       |
| `@hairy/utils`          | 1.46.0  | 1.47.0  | Minor 更新，自有库。       |
| `@types/react`          | 19.1.3  | 19.2.14 | Minor 更新。               |
| `@types/react-dom`      | 19.1.3  | 19.2.3  | Minor 更新。               |
| `typescript`            | 5.8.3   | 5.9.3   | Minor 更新，通常向后兼容。 |

---

## 建议升级策略

### 第一阶段：安全补丁 + 低风险（立即执行）

1. `next` 16.0.8 → 16.1.6（安全补丁，优先级最高）
2. 所有 🟢 低风险 patch/minor 更新（批量升级）
3. `react` / `react-dom` patch 更新

### 第二阶段：中风险更新（需验证）

1. `framer-motion`、`viem` minor 更新
2. `eslint-plugin-*` 系列更新
3. `etherlib-generator` 更新（需验证代码生成结果）

### 第三阶段：Major 升级（需专项处理，各自独立分支）

1. **ESLint 生态链**：`eslint` 10 + `@antfu/eslint-config` 7 + 相关插件（作为一组升级）
2. **Tailwind CSS v4 迁移**：`tailwindcss` 4 + `tailwind-variants` 3 + `@heroui/theme` peer 修复
3. **wagmi v3 迁移**：`wagmi` 3 + `viem` 最新 + `@rainbow-me/rainbowkit` 兼容性验证
4. **工具链**：`@hairy/lnv` 9 + `unplugin-auto-import` 21 + `vitest` 4

### 前置条件

- ⚠️ 修复主分支 SSG 构建失败（`localStorage` SSR 问题）
- ⚠️ 处理 Next.js 16 Turbopack 迁移（或明确使用 `--webpack` 模式）
