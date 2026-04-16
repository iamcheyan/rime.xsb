# Rime Quanpin Config

这是当前使用中的 Rime 配置仓库，现阶段已经从早期的 `sbzr`/扩展联动结构，收敛为一套以全拼为主的精简方案集。

当前主线目标：

1. 维护一套基于 `rime-ice` 词库的中文全拼方案。
2. 保留一个中日英混输方案，中文体验尽量与纯全拼一致。
3. 保留日语罗马字输入方案 `jaroomaji`。

仓库当前以“能稳定运行的现状”为准，不再把旧的 `resource/`、`sbzr`、Chrome 扩展结构视为主路径。

## 当前方案

- `pinyin.schema.yaml`
  纯中文全拼主方案。
- `pinyin_mix.schema.yaml`
  中日英混输方案。
- `jaroomaji.schema.yaml`
  日语罗马字输入方案。
- `default.custom.yaml`
  方案列表、切换器和全局按键补丁。

当前默认方案列表在 [default.custom.yaml](/mnt/c/Users/hkaku/AppData/Roaming/Rime/default.custom.yaml:1) 中维护，现用方案是：

- `pinyin`
- `pinyin_mix`
- `jaroomaji`

## 词库结构

### 中文词库

[pinyin.dict.yaml](/mnt/c/Users/hkaku/AppData/Roaming/Rime/pinyin.dict.yaml:1) 只做聚合导入，当前导入：

- `dicts.cn/8105`
- `dicts.cn/base`
- `dicts.cn/ext`
- `dicts.cn/tencent`
- `dicts.cn/others`

这些词库位于 `dicts.cn/`，主要来源于 `rime-ice` 的中文词库拆分。

### 英文词库

- `easy_en.dict.yaml`
- `dicts.en/easy_en.dict.yaml`
- `dicts.en/easy_en.extra.dict.yaml`

英文方案使用 `easy_en`，额外补充词条放在 `dicts.en/easy_en.extra.dict.yaml`。

### 日文词库

- `jaroomaji.dict.yaml`
- `dicts.jp/jaroomaji.user.dict.yaml`
- `dicts.jp/jaroomaji.kana_kigou.dict.yaml`
- `dicts.jp/jaroomaji.mozc.dict.yaml`
- `dicts.jp/jaroomaji.jmdict.dict.yaml`
- `dicts.jp/jaroomaji.mozcemoji.dict.yaml`
- `dicts.jp/jaroomaji.kanjidic2.dict.yaml`

`jaroomaji.dict.yaml` 负责聚合 `dicts.jp/` 下的日文词典。

## 混输方案说明

[pinyin_mix.schema.yaml](/mnt/c/Users/hkaku/AppData/Roaming/Rime/pinyin_mix.schema.yaml:1) 是当前的混输方案。

现状：

- 中文部分复用 `pinyin` 的主词典 `pinyin`
- 中文 translator 使用 `script_translator`
- 中文 `speller/algebra` 与纯 `pinyin` 对齐
- 保留英文 `easy_en_mix`
- 保留日文 `jp_mix`
- 保留 `lua_filter@mix_recent_filter` 处理最近输入历史

因此，混输模式下的中文词库来源与纯拼音一致，差异主要来自多 translator 并行时的排序与混排行为，而不是中文词库缺失。

## 当前行为

### pinyin

- 候选页大小为 `6`
- 选择键为 `Space + 1 2 3 4 5`
- `Tab` / `Shift+Tab` 在候选间移动
- `Shift+BackSpace` 清空当前输入
- 开启 `history_translator`
- 开启整句和造词
- 开启 completion
- `max_code_length: 0`

### pinyin_mix

- 继承与 `pinyin` 接近的中文输入体验
- 同时挂载英文和日文 translator
- 适合中日英混合输入

## 常用维护命令

### 重新部署 Rime

```bash
./rebuild.sh
```

`rebuild.sh` 会转调 `rebuild`，用于清理缓存并重新部署当前配置。

### 提交并推送当前配置

```bash
./push
```

`push` 会自动整理 Git 状态、生成提交信息并推送当前分支。

### 安装日语词典

```bash
./scripts/install-jaroomaji-dicts.sh
```

该脚本会下载缺失的 `jaroomaji` 词典文件到 `dicts.jp/`，并尝试更新 `default.custom.yaml`。

## 清理状态

仓库中仍可能残留一些历史文件或兼容脚本，例如旧的 `sbpy`、`sbzr`、`resource` 相关内容。它们不再属于当前主线结构。

处理原则：

- 文档、配置和目录结构不一致时，以当前可运行配置为准
- 新增或修改中文词库时，优先更新 `dicts.cn/`
- 新增或修改英文词库时，优先更新 `dicts.en/`
- 新增或修改日文词库时，优先更新 `dicts.jp/`
- 避免继续引入新的 `resource/` 路径引用

## 当前定位

这不是一个功能堆叠型 Rime 仓库，而是一套围绕 `全拼 + 混输 + 日语输入` 维护的个人配置。

重点是：

- 词库路径清晰
- 配置可直接部署
- 混输中文体验与纯全拼尽量一致
- 日文词典可独立维护
