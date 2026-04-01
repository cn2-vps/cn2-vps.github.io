# Skill: traffic-reporter
# 功能：每日汇报各AFF推广页的访问量统计

trigger:
  - "报一下今天访问量"
  - "生成流量日报"
  - "VPS站点访问统计"
  - cron: "0 9 * * *"  # 每天上午9点执行

parameters:
  date:
    type: string
    description: 统计日期，默认昨天
    default: "yesterday"

requirements:
  - 需要先配置Cloudflare Web Analytics
  - 需要Cloudflare API Token

tools:
  - exec
  - read
  - write

workflow:
  1. 检查配置:
     - 检查是否有Cloudflare API Token
     - 检查site_token是否已配置到页面中
  
  2. 获取统计数据:
     - 方法A：使用Cloudflare Analytics API
       API: GET /zones/{zone_id}/analytics/dashboard
     - 方法B：如果API不可用，读取本地access.log（如有配置nginx日志）
  
  3. 整理数据:
     - 总访问PV/UV
     - 各厂商详情页的访问量排名
     - 访问来源（搜索引擎/直接访问/推荐）
     - 访问地域分布
  
  4. 生成报告:
     - 保存到：/root/.openclaw/workspace/cn2-vps-site/stats/daily/{date}.md
     - 同时输出到控制台供汇报
  
  5. 可选：发送报告:
     - 如配置了通知渠道，发送日报给用户

report_template:
  format: markdown
  sections:
    - 日期
    - 总体数据（PV/UV/跳出率/平均停留时间）
    - 各页面访问量排名
    - 本周趋势对比
    - 转化分析（点击AFF链接的次数，如有追踪）

note: |
  由于GitHub Pages是静态托管，无法直接获取访问日志。
  需要配合Cloudflare Web Analytics或其他第三方统计工具。
  
  配置步骤：
  1. 注册Cloudflare账号
  2. 添加域名（或使用xxx.github.io默认域名）
  3. 开启Web Analytics（免费）
  4. 获取Site Token
  5. 将Token填入各页面的统计代码中
  6. 配置Cloudflare API Token用于自动拉取数据

current_status: |
  页面已预留Cloudflare统计代码位置（PLACEHOLDER_TOKEN）。
  待用户配置Token后，统计功能生效。
