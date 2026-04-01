# Skill: aff-page-creator
# 功能：根据VPS厂商AFF链接自动创建推荐页面

trigger:
  - "给我生成一个VPS推荐页面"
  - "帮我写个厂商推荐"
  - "新增VPS推荐"
  - "创建aff推广页"

parameters:
  provider_name:
    type: string
    description: 厂商名称
    required: true
  aff_link:
    type: string
    description: AFF推广链接
    required: true
  website:
    type: string
    description: 厂商官网
    required: true
  category:
    type: string
    description: 分类（国内厂商/海外厂商/CN2 GIA/低价VPS）
    required: true

tools:
  - kimi_search
  - write
  - edit
  - exec

workflow:
  1. 搜索厂商信息:
     - 使用kimi_search搜索厂商的最新价格、配置、线路特点
     - 搜索关键词："{provider_name} VPS 2026 价格 配置 线路"
  
  2. 创建详情页面:
     - 路径：/root/.openclaw/workspace/cn2-vps-site/vps/{provider_id}.html
     - 使用简约朴实的风格（参考已有页面）
     - 内容包含：
       * 厂商基本信息
       * 优缺点分析（诚实写缺点）
       * 热门套餐表格
       * 适合人群
       * 购买建议
       * AFF链接按钮
       * Cloudflare统计代码
  
  3. 更新首页:
     - 在index.html的"最近更新"表格中添加新厂商
  
  4. 更新分类页面:
     - 根据category更新对应的分类页面（domestic.html/foreign.html/cn2-gia.html/budget.html）
  
  5. 更新providers.json:
     - 添加新厂商的基本信息
  
  6. 部署:
     - git add -A
     - git commit -m "新增{provider_name}推荐页面"
     - git push origin main

style_guide:
  - 降低"AI味"：
    * 不要用"首先/其次/综上所述"
    * 文字口语化，像个人站长写的
    * 诚实写缺点，不要一味吹捧
    * 适当用emoji和括号吐槽
  
  - 页面结构：
    * 简单头部，返回首页链接
    * 白色背景，黑色文字，蓝色链接
    * 表格展示价格和配置
    * 底部AFF声明
  
  - SEO优化：
    * title包含厂商名+价格+关键词
    * meta description写清楚卖点
    * 使用语义化标签

example_pages:
  - rainyun.html (雨云)
  - racknerd.html (RackNerd)
  - sixtynet.html (六六云)
  - aliyun.html (阿里云)
