核心审计流程:
查候选 Story
从 Azure DevOps 查 BA 列里的 User Story（未关闭）。
去重
先读该 Story 的评论。
有 <!-- WDF101-AUDIT-AGENT --> 就跳过。
检查 1：Parent Link（结构）
Story 必须有父级，且父级是 Feature/Epic。
失败：立即发评论并结束该 Story。
检查 2：Spec Doc（结构）
在父级关系里找 *-spec-v*.docx 附件，且能下载非空。
失败：立即发评论并结束。
检查 3：Mockups Linked（结构）
在父级关系里找 *-mock*.html（超链接或附件）。
失败：立即发评论并结束。
检查 4-6（质量，不提前退出）
Cross-Ref：Spec 的 Accompanying Files 与父级 mockup 是否一致。
Valid：每个 mockup HTML 是否可下载、非空、且有交互元素。
Story Ref：Story Description 是否有 Spec Reference: xxx § yyy。
生成分数与结果
汇总每项 PASS/WARN/FAIL。
形成审计表格 + Chain Score。
发评论并终端打印状态
把结果发到 Discussion。
终端实时输出：POSTING/POSTED/SKIPPED/NOT POSTED。

