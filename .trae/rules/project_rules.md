1. Keep good file structure and modularity
   - Keep files in logical orders, separate modules to different files or even folders when necessary.
   - Organize code into logical modules with clear responsibilities
   - Use appropriate design patterns for the project's architecture
   - Follow the single responsibility principle where possible
   - Maintain consistent naming conventions for files and directories
   - **File Size Limit**: Do not recommend a single file to exceed 200 lines (not including comments or docstrings).
   - **Directory Structure**: Create folders, subfolders, scripts, or even subscripts accordingly to ensure granularity and organization.

2. Do not break old implementation while modifying or adding new features. For example, do not remove a lot of original html code if new functionality is added, which gives new features to the established visuals.
   - Preserve existing behavior when adding new features
   - Test changes thoroughly to ensure backward compatibility
   - Make incremental changes rather than massive rewrites when possible
   - Document any necessary breaking changes explicitly

3. Do not use placeholders, implement the functions faithfully.
   - Do not use TODO comments or stub implementations in final code
   - Provide complete implementations for all features
   - Ensure all edge cases are handled properly

4. Follow consistent code style and best practices
   - Maintain consistent indentation, spacing, and formatting
   - Use meaningful variable and function names
   - Add appropriate comments for complex logic
   - Follow language-specific style guides and conventions

5. Ensure proper error handling and validation
   - Validate all inputs and parameters
   - Implement comprehensive error handling
   - Provide meaningful error messages
   - Fail gracefully when errors occur

6. Optimize for performance and maintainability
   - Write efficient code that minimizes computational complexity
   - Avoid unnecessary dependencies
   - Use appropriate data structures and algorithms
   - Consider potential performance bottlenecks

7. Write testable code
   - Design components to be easily testable
   - Create unit tests for critical functionality
   - Separate concerns to enable isolated testing
   - Consider test-driven development where appropriate

8. Document your code thoroughly
   - Include clear function documentation with parameters and return values
   - Document complex business logic
   - Add README files to explain component purpose and usage
   - Keep documentation up-to-date with code changes
   - Include and maintain top-level docs in both English and Chinese:
      - The documentation structure must be:
        docs/
          en/
            README.md: Describe the app's need/problem, user types/personas, and core functionality.
            TODO.md: Keep an organized, prioritized task list grouped by module/feature.
            BUGS.md: Maintain a concise bug book capturing symptoms, minimal repro steps, root cause analysis, and the eventual fix/solution.
          cn/
            README.md: Chinese translation of the above.
            TODO.md: Chinese translation of the above.
            BUGS.md: Chinese translation of the above.
      - Always keep these files up to date as the app evolves, ensuring both languages are synchronized for every update.

9. Implement robust security practices
   - Never expose API keys or sensitive credentials in client-side code
   - Implement proper authentication and authorization for all API endpoints
   - Use Row Level Security (RLS) policies to enforce access control at the database level
   - Validate and sanitize all user inputs to prevent injection attacks
   - Implement proper session management and secure cookie handling
   - Follow the principle of least privilege when designing access controls

10. Maintain and follow project guidelines
    - Regularly review and update documentation to reflect current implementation
    - Ensure all team members follow established guidelines consistently
    - Keep all guidelines in sync across different documents
    - Verify implementation against guidelines during code reviews
    - Propose improvements to guidelines based on practical implementation experience

11. Maintain comprehensive documentation in README.md
    - Always maintain complete file structure with annotations
    - Document design logic and architecture decisions
    - Include detailed user flow with function descriptions
    - For API endpoints, must include:
      - Payload structure with examples
      - Response structure with examples
      - Example curl commands for testing
      - Reference to the file that implements the endpoint
    - For web application design documentation:
      - Include a complete mapping between database tables/columns, backend endpoints, and frontend interfaces
      - Present mappings in tabular format for clarity
      - List all related files in a structured hierarchy
      - Document data flow between components
      - Include type definitions for all interfaces
      - Provide relationship diagrams between database entities
      - Document authentication and authorization flows

12. Database migration standards
    - For migration SQL files, provide detailed comments for all database objects, including tables, columns, functions, and triggers.
    - Create indices on frequently queried columns to ensure optimal query performance.
    - Carefully design Row Level Security (RLS) policies to avoid recursion.
    - Document the purpose and impact of each migration.
    - Include rollback instructions where applicable.
    - **Extendable Schema**: Always add a `meta` column of type `jsonb` to tables to ensure future extendability without requiring schema changes.

13. Supabase schema organization
    - Use custom schemas in Supabase for better organization
    - Apply custom schemas to all functions, tables, and triggers
    - Avoid using the default public schema when possible
    - Maintain clear naming conventions for schema objects
    - Create schemas with: CREATE SCHEMA schema_name;
    - Expose schemas in API settings and grant proper permissions:
      GRANT USAGE ON SCHEMA schema_name TO anon, authenticated, service_role;
      GRANT ALL ON ALL TABLES IN SCHEMA schema_name TO anon, authenticated, service_role;
      GRANT ALL ON ALL ROUTINES IN SCHEMA schema_name TO anon, authenticated, service_role;
      GRANT ALL ON ALL SEQUENCES IN SCHEMA schema_name TO anon, authenticated, service_role;
      ALTER DEFAULT PRIVILEGES FOR ROLE postgres IN SCHEMA schema_name GRANT ALL ON TABLES TO anon, authenticated, service_role;
      ALTER DEFAULT PRIVILEGES FOR ROLE postgres IN SCHEMA schema_name GRANT ALL ON ROUTINES TO anon, authenticated, service_role;
      ALTER DEFAULT PRIVILEGES FOR ROLE postgres IN SCHEMA schema_name GRANT ALL ON SEQUENCES TO anon, authenticated, service_role;

14. Security-first database access
    - Prioritize RLS (Row Level Security) with anon key for client access
    - Avoid service role key unless absolutely necessary (service role is dangerous)
    - Use JWT key as an alternative secure option
    - Create different clients for different key types
    - Call the appropriate client based on security context and requirements
    - Always validate permissions at the database level

15. Docker Compose environment management
    - Always use a .env file for Docker Compose configurations
    - Never hardcode sensitive values directly in docker-compose.yml
    - Use environment variable substitution syntax: ${VARIABLE_NAME:default_value}
    - Provide a .env.example file with sample values for all required variables
    - Add .env to .gitignore to prevent committing sensitive data
    - Document all environment variables and their purposes in README.md
    - Use consistent naming conventions for environment variables (e.g., UPPERCASE_WITH_UNDERSCORES)

16. Case convention handling between backend and frontend
    - Always check for both snake_case and camelCase when connecting backend and frontend
    - Ensure proper data transformation between database conventions (typically snake_case) and frontend conventions (typically camelCase)
    - Implement consistent serialization/deserialization utilities to handle case conversions
    - Validate that all properties are correctly mapped regardless of naming convention
    - Document the case convention strategy in API documentation
    - Consider using libraries or middleware that automatically handle case transformations

17. Frontend style isolation (Tailwind/CSS) for multi-app modules
    - Use ONE global Tailwind entry that declares layers:
      - src/index.css contains only: `@tailwind base; @tailwind components; @tailwind utilities;`
      - Do NOT repeat these in app-level CSS files.
    - App-level styles:
      - Create an `index.css` inside each app (e.g., `apps/smartab/index.css`).
      - Do NOT use `@tailwind` in app CSS. Prefer plain CSS or `@layer components` if configured.
      - Namespace all custom classes with an app prefix (e.g., `.smartab-…`, `.bioseq-…`). Avoid generic class names.
      - Avoid global element selectors (e.g., `body`, `a`, `button`) in app CSS. If necessary, scope under an app root container (e.g., `#app-smartab …`).
    - Tailwind content scanning:
      - Ensure `tailwind.config.js` includes all app paths, e.g.: `content: ['./index.html', './src/**/*.{js,ts,jsx,tsx}', './apps/**/**/*.{js,ts,jsx,tsx,html}']`.
      - This prevents unused utilities from different apps being purged incorrectly and avoids CSS bloat.
    - Utilities vs. custom styles:
      - Use Tailwind utilities for layout and spacing.
      - Use app-namespaced custom classes only for repeated app-specific patterns (cards, icons, etc.).
    - Strict isolation options (when embedding multiple apps on the same page):
      - Build separate CSS bundles per app with per-app Tailwind configs, OR
      - Configure a Tailwind `prefix` per app (e.g., `prefix: 'sb-'`). Do not mix multiple prefixes in one shared bundle.
    - Theming and tokens:
      - Keep shared design tokens as CSS variables under `:root` or a shared layer.
      - For app-specific tokens, scope variables under the app root container to avoid global collisions.
    - Linting/build:
      - If the linter flags PostCSS directives (`@tailwind`, `@apply`), use plain CSS in app files or configure the linter to ignore those files.
      - Import the app CSS only from the app entry file to avoid leaking styles globally.

18. Annotations (注解), docstrings (文档字符串) and documentations (文档)
    - **Language Requirement (语言要求)**: All documentation, including docstrings, comments (注释), and field descriptions (字段描述), must be provided in both English and Chinese.
    - Pydantic/Schema annotations (Pydantic/Schema注解) (REQUIRED):
      - All Pydantic models or schema classes must annotate every field with explicit type hints (类型提示) and a `Field(..., description=...)` (或同等语法) to surface meanings in API docs.
      - Use upstream key names for API-facing models (如 camelCase)，并按需包含 aliasing (别名) 或 serializers (序列化器)；在模型 docstring 中记录映射策略 (映射策略)。
      - Prefer `Config.extra` set intentionally (`allow`/`forbid`) and document why (说明原因)。

    - Module-level docstring (模块级文档字符串) (REQUIRED at the top of every script in any language):
      - References (引用): list the reference snippets, functions, and files used to implement the script (internal and external), e.g., upstream spec files or internal helpers.
      - Overview (概述): one paragraph explaining what the script/module does and which main classes or functions it exposes.
      - Security/performance considerations (安全/性能考虑) when relevant.

    - Class and function docstrings (类和函数文档字符串) (REQUIRED):
      - Cover purpose (目的), behavior (行为), parameters (参数：名称、类型、语义), return types (返回类型), raised exceptions (抛出异常), and a short example (示例) when non-trivial.
      - Include “References” (引用) subsection listing source functions/snippets/files used to implement this object.
      - Include “Consumers” (调用方) subsection listing files and modules that are expected to call/import this object.
      - Keep style consistent (保持风格一致) (Google 或 NumPy 样式 for Python; JSDoc for TS/JS; block comments for other languages)。

    - OpenAPI visibility (OpenAPI 可见性):
      - When exposing endpoints (端点), always attach `response_model` (响应模型) (or language/framework equivalent) and ensure models contain field descriptions (字段描述) so docs show meanings for each response term.
      - Keep route `summary` (摘要) and parameter docs (参数文档) accurate and localized (本地化) as needed.

    - Cross-language guidance (跨语言指南) (apply regardless of language):
      - Python: type hints (类型提示) are mandatory; docstrings for modules, classes, and functions required.
      - TypeScript/JavaScript: use JSDoc with explicit types (显式类型); document params/returns (参数/返回) and references/consumers (引用/调用方)。
      - SQL: use `COMMENT ON` statements for tables/columns/functions and top-of-file block comment with references and consumers.
      - Shell/CLI: top-of-file header (文件头部说明) describing purpose (目的), usage (用法), inputs/outputs (输入/输出), references (引用), and consumers (调用方)。

    - PR acceptance criteria (PR 验收标准):
      - Changes introducing or altering public APIs, models, or functions must include the above annotations/docstrings (以上注解/文档字符串)。
      - Reviews should block if module/class/function docstrings or Pydantic field descriptions are missing for user-facing schemas (用户端模式)。

    - Minimal example (Python/Pydantic) / 最小示例（Python/Pydantic）:
      """
      References / 参考:
        - external/erplus/docs/api/get_programs.md
        - app/models/erplus/programs.py: ProgramsListResponse
      File / 文件:
        - backend/app/models/erplus/programs.py
      Overview / 概述:
        Exposes ProgramsListResponseModel used by /programs route.
        提供 /programs 路由使用的 ProgramsListResponseModel。
      """
      from pydantic import BaseModel, Field

      class ExampleModel(BaseModel):
          """
          Example model for programs list response.
          示例模型：用于 programs 列表响应。

          Consumers / 调用方:
            - backend/app/api/routes/programs.py
          """
          id: str = Field(
              ...,
              description="Primary identifier / 主键标识"
          )
          name: str = Field(
              ...,
              description="Display name / 显示名称"
          )

      def do_work(x: int) -> str:
          """
          Do work and return a string.
          执行任务并返回字符串。

          Args / 参数:
            x (int): Input number / 输入数字。

          Returns / 返回:
            str: Result string / 结果字符串。

          Raises / 异常:
            ValueError: If x < 0. / 当 x < 0 时抛出。

          References / 参考:
            - app/utils/math.py: helper_sum

          Consumers / 调用方:
            - app/api/routes/example.py
          """
          if x < 0:
              # Forbid negative input by raising an exception
              # 禁止负数输入，抛出异常
              raise ValueError("x must be non-negative")
          return str(x)

19. Frontend client-state item/detail pages (no new routes)
   - Do not add new routes (e.g., `/items/:id`) for item/detail views. Render details within the existing route using client-side state.
   - Store the selected item identity and view mode in centralized client state (e.g., Redux/Zustand/Context) to avoid prop drilling.
   - Fetch data reactively when selection changes:
      - Cancel in-flight requests on id change or unmount; debounce rapid changes when applicable.
      - Normalize and cache records by `id` in state; reuse fresh cache to minimize refetches.
   - Keep the URL stable:
      - Do not change the route; if navigation history is desired, use `history.pushState/replaceState` without altering the path.
   - UX behavior:
      - Use skeletons while loading and clear error states with retry actions.
      - Preserve list scroll position, filters, and sort state when opening/closing details.
   - Architecture:
      - Isolate detail UI into a dedicated component/module; keep the container responsible for state, effects, and data orchestration.
      - Preserve existing DOM/HTML; add detail panels/modals/split panes as overlays without removing established visuals.
   - Data contracts and security:
      - Validate item ids; handle snake_case↔camelCase mapping at boundaries (see rule 16).
      - Use anon-key clients with RLS for data access; never expose privileged keys in the client.
   - Documentation:
      - Document state shape, loading lifecycle, caching, and error handling in README, with example snippets.


20. Avoid RLS recursion with junction tables and helper functions
   - Model relationships using an info table plus a junction table to prevent RLS policies from recursively referencing the same table graph.
   - Provide narrowly scoped helper functions (SECURITY DEFINER where appropriate) to read minimal required info while bypassing RLS safely; validate inputs and avoid privilege escalation.
   - Qualify helpers with explicit schema, grant least privileges, and add `COMMENT ON FUNCTION` documentation; log/audit calls when feasible.
   - Test policies specifically for recursion risks; use `EXPLAIN` with RLS enabled to verify query plans.

21. Handle race conditions in high-frequency CRUD tables
   - Wrap write operations in transactions with appropriate isolation (typically READ COMMITTED; escalate to REPEATABLE READ only when required).
   - Use unique indexes and UPSERT patterns; apply optimistic locking via a `version` or `updated_at` check in WHERE clauses.
   - Introduce idempotency keys for externally triggered writes; consider advisory locks for short critical sections only.
   - Implement bounded retries with backoff for transient serialization/deadlock errors; instrument contention metrics.

22. Maintain TODO.md from project inception
   - Create and keep a root `TODO.md` as the single source of truth for planned work from day one.
   - Prefer `TODO.md` over in-code TODO comments; keep items short, verb-led, and prioritized with owners where applicable.
   - Link items to issues/PRs; review and prune regularly; include clear acceptance criteria.
   - Organize by app/module sections to reflect the repository structure; update alongside related PRs.

23. Molecule viewer standard
   - Default to NGL viewer: `https://cdn.jsdelivr.net/npm/ngl/dist/ngl.js`.
   - Fallback to 3Dmol.js when NGL is unavailable: `https://3dmol.org/build/3Dmol-min.js`.
   - Feature-detect and lazy-load the viewer; isolate styles and avoid altering existing DOM structure.
   - Make CDN URLs configurable; document initialization patterns and expected data formats (e.g., PDB, SDF).

24. Bypass Supabase API row limits for aggregation and bulk operations
   - Never use client-side `data.length` for counting total records; use `count: 'exact', head: true` or server-side views/RPCs.
   - For bulk updates/transfers, pass parameters (e.g., quantity/filters) to an RPC function instead of fetching and iterating over thousands of IDs on the client.
   - Use SQL Views or Materialized Views for complex aggregations (e.g., user balances) to return a single pre-calculated row.
   - When large datasets must be fetched, implement cursor-based pagination using `range()` or `limit()/offset()` rather than requesting all rows at once.

25. Frontend Design and UI/UX Standards
   - **No Emojis**: Do not use emojis in the UI. Always use professional icons (e.g., shadcn, lucide-react, or similar packages).
   - **Canvas Applications**: For canvas-like features, always track both relative and absolute locations of elements (divs) to ensure correct positioning across different contexts.
   - **Theme Support**: Always implement Day/Night (Light/Dark) color themes. Ensure every update maintains the theme switching functionality.
   - **Internationalization (i18n)**: Always implement English/Chinese (en/cn) language switching. Ensure every update maintains the language switching functionality.
   - **Responsive Design**: Ensure content is adaptable to different devices including Phones, Tablets, and PCs with different view sizes. Test layouts on multiple breakpoints.
   - **Edit Functionality (编辑功能)**: When implementing edit functionality, upon successful submission:
     - Emit a success event/callback to notify the user of the successful operation.
     - Refresh or invalidate the parent component's data to allow users to immediately see the updated changes without manual refresh.
     - Use optimistic updates where appropriate for better UX, with rollback on failure.
   - **Modal/Dialog Sizing (模态框/对话框尺寸)**:
     - Modals and dialogs must never exceed the client viewport dimensions; always constrain with `max-height: 80vh` and `max-width: 80vw` (or similar ratios).
     - Apply `overflow-y: auto` to the scrollable content area, not the entire dialog, so headers, footers, and action buttons remain fixed and visible.
     - Separate fixed vs. scrollable regions: keep dialog title bar and primary action buttons always visible; allow only the body/content section to scroll.
     - Avoid full-screen dialogs unless explicitly required (e.g., media viewers). Prefer ratios like 80% width for large content, 50%-60% for forms, and smaller for confirmations.
     - On smaller viewports, use responsive breakpoints to adjust modal size gracefully (e.g., switch from centered modal to bottom sheet on mobile).
     - Always test modal behavior with varying content lengths to ensure scrollbars appear correctly and no content is clipped or inaccessible.

26. Async Service Concurrency Management (异步服务并发管理)
   - **Concurrency Control (并发控制)**:
     - Use semaphores (`asyncio.Semaphore` in Python, or equivalent in other languages) to limit concurrent operations for external API calls, database connections, and resource-intensive tasks.
     - Define explicit concurrency limits per service type (e.g., `MAX_CONCURRENT_API_CALLS`, `MAX_DB_CONNECTIONS`, `MAX_WORKER_TASKS`).
     - Implement connection pooling where applicable; respect pool size limits and handle exhaustion gracefully.
   - **Configuration Management (配置管理)**:
     - Store all concurrency-related parameters in environment files (`.env`) or dedicated config files/scripts (e.g., `config.py`, `settings.yaml`).
     - Never hardcode concurrency limits; use environment variables with sensible defaults: `int(os.getenv("MAX_CONCURRENT_REQUESTS", "10"))`.
     - Document all concurrency parameters in README with recommended values for development, staging, and production.
   - **Resource Lifecycle (资源生命周期)**:
     - Always acquire and release semaphores using context managers (`async with semaphore:`) to prevent leaks.
     - Implement proper cleanup on shutdown; cancel pending tasks and close connections gracefully.
     - Use timeouts for semaphore acquisition to detect deadlocks: `async with asyncio.timeout(30):`.
   - **Error Handling (错误处理)**:
     - Handle `asyncio.TimeoutError` and resource exhaustion errors explicitly with meaningful logs.
     - Implement retry logic with exponential backoff for transient failures; respect rate limits.
     - Track and monitor concurrency metrics (active tasks, queue depth, wait times) for observability.
   - **Best Practices (最佳实践)**:
     - Prefer bounded queues (`asyncio.Queue(maxsize=N)`) over unbounded to prevent memory exhaustion.
     - Use task groups or `asyncio.gather` with proper exception handling; avoid fire-and-forget patterns.
     - Test concurrent code under load to identify race conditions, deadlocks, and resource starvation.

27. Supabase Storage Media Handling (Supabase存储媒体处理)
   - **Dual-Upload Strategy (双重上传策略)**:
     - For every image upload, store both the original file and a compressed preview version.
     - Original files go to a primary path (e.g., `bucket/originals/{uuid}.{ext}`); previews go to a parallel path (e.g., `bucket/previews/{uuid}.{ext}`).
     - Use consistent naming conventions linking originals to previews (same UUID or filename prefix).
   - **Image Compression (图片压缩)**:
     - **Size Limit**: Preview images must not exceed 128KB; adjust quality and resolution dynamically to meet this constraint.
     - Generate preview images at reduced resolution (e.g., max 300px width/height) with aggressive compression (JPEG quality 30-50, WebP quality 40-60).
     - Use efficient libraries: `sharp` (Node.js), `Pillow` (Python), or browser-native `canvas` API for client-side compression.
     - Implement adaptive quality: start at low quality, check file size, and further reduce if needed to stay under 128KB.
     - Preserve aspect ratio; prefer WebP format for better compression-to-quality ratio; fall back to JPEG for compatibility.
     - For large images, resize first (e.g., max 300px longest edge) before applying quality compression to ensure size limit is met.
   - **Video Handling (视频处理)**:
     - Extract a thumbnail frame from videos (e.g., at 1 second) as the preview image.
     - Store video originals and thumbnail previews in separate paths (e.g., `bucket/videos/{uuid}.mp4`, `bucket/video-thumbnails/{uuid}.jpg`).
     - Consider generating lower-resolution video previews for bandwidth-sensitive contexts (optional).
   - **Retrieval Strategy (检索策略)**:
     - Always fetch preview images for list views, galleries, and thumbnails to minimize bandwidth.
     - Load original/full-resolution media only on explicit user action (e.g., click to enlarge, download).
     - Use signed URLs with appropriate expiration for private buckets; cache public URLs where applicable.
   - **Database Schema (数据库模式)**:
     - Store both `original_url` and `preview_url` (or paths) in the database record.
     - Include metadata: `file_size`, `dimensions` (width/height), `mime_type`, `created_at`.
     - Add a `meta` JSONB column for extensibility (e.g., EXIF data, compression settings).
   - **Upload Flow (上传流程)**:
     - Client-side: Compress preview before upload when feasible to reduce server load.
     - Server-side: Validate file types and sizes; generate preview if not provided by client.
     - Use transactions or atomic operations to ensure both original and preview are uploaded successfully.
   - **Error Handling (错误处理)**:
     - Handle upload failures gracefully; retry with exponential backoff; clean up partial uploads.
     - Validate that preview generation succeeded before marking upload as complete.
     - Log compression metrics (original size vs. preview size) for monitoring storage savings.

28. Supabase Query Result Handling: single() vs maybeSingle() (Supabase查询结果处理)
   - **Understanding the Methods (方法理解)**:
     - `single()`: Expects exactly ONE row. Throws `PGRST116` error if 0 rows or multiple rows returned.
     - `maybeSingle()`: Expects 0 or 1 row. Returns `null` for 0 rows; throws error if multiple rows returned.
     - Default (no modifier): Returns an array of rows, even for 0 or 1 result.
   - **When to Use single() (何时使用single())**:
     - Use when the query MUST return exactly one row (e.g., fetching by primary key where existence is guaranteed).
     - Appropriate after INSERT/UPDATE with `.select()` when you expect the modified row back.
     - Use for lookups where absence indicates a bug or invalid state that should fail loudly.
   - **When to Use maybeSingle() (何时使用maybeSingle())**:
     - Use when 0 or 1 row is valid (e.g., checking if a record exists, optional relationships).
     - Preferred for user-initiated lookups where "not found" is a normal condition, not an error.
     - Use for queries with filters that may or may not match (e.g., `where('email', 'eq', userEmail)`).
   - **Error Handling Patterns (错误处理模式)**:
     - Always check both `error` and `data` after queries:
       ```javascript
       const { data, error } = await supabase.from('users').select().eq('id', id).maybeSingle();
       if (error) throw error;
       if (!data) return notFound(); // Handle missing record gracefully
       ```
     - For `single()`, catch the specific error code `PGRST116` if you need custom handling.
     - Never assume `data` is non-null after `maybeSingle()` without explicit checks.
   - **Common Pitfalls (常见陷阱)**:
     - Using `single()` for queries that may legitimately return 0 rows causes runtime errors.
     - Forgetting that `maybeSingle()` still errors on multiple rows—ensure your query is properly constrained.
     - Using neither on queries expected to return one row, then accessing `data[0]` without length checks.
   - **Best Practices (最佳实践)**:
     - Prefer `maybeSingle()` as the safer default; use `single()` only when absence is truly exceptional.
     - Add unique constraints to columns used in `single()`/`maybeSingle()` queries to prevent multi-row errors.
     - Document in code comments why `single()` is safe when used (e.g., `// PK lookup, guaranteed unique`).
     - In TypeScript, leverage return types: `single()` returns `T`, `maybeSingle()` returns `T | null`.

29. Supabase Vector Extension and halfvec Usage (Supabase向量扩展与halfvec使用)
   - **Extension Setup (扩展设置)**:
     - Enable the vector extension in the `public` schema for best compatibility with Supabase clients and RPC functions:
       ```sql
       CREATE EXTENSION IF NOT EXISTS vector WITH SCHEMA public;
       ```
     - Exposing to `public` ensures vector types (`vector`, `halfvec`, `sparsevec`) are accessible without schema qualification.
     - Avoid creating the extension in custom schemas as it may cause type resolution issues with PostgREST.
   - **halfvec Usage (halfvec使用)**:
     - Use `halfvec` (16-bit floating point) instead of `vector` (32-bit) for storage-efficient embeddings when precision loss is acceptable.
     - `halfvec` reduces storage by ~50% and improves query performance for large vector datasets.
     - Declare columns with explicit dimensions: `embedding halfvec(1536)` for OpenAI embeddings, `halfvec(384)` for smaller models.
   - **Indexing (索引)**:
     - Create HNSW or IVFFlat indexes for similarity search:
       ```sql
       CREATE INDEX ON items USING hnsw (embedding halfvec_cosine_ops);
       ```
     - Choose appropriate operator class: `halfvec_cosine_ops`, `halfvec_l2_ops`, or `halfvec_ip_ops` based on similarity metric.
     - Tune index parameters (`m`, `ef_construction` for HNSW) based on dataset size and recall requirements.
   - **RPC Functions (RPC函数)**:
     - When creating similarity search functions, cast input vectors explicitly:
       ```sql
       CREATE FUNCTION match_items(query_embedding halfvec(1536), match_count int)
       RETURNS SETOF items AS $$
         SELECT * FROM items
         ORDER BY embedding <=> query_embedding
         LIMIT match_count;
       $$ LANGUAGE sql STABLE;
       ```
     - Grant execute permissions to appropriate roles: `GRANT EXECUTE ON FUNCTION match_items TO authenticated;`
   - **Client Integration (客户端集成)**:
     - Pass embedding arrays directly from client; Supabase/PostgREST handles type conversion.
     - For TypeScript, define embedding fields as `number[]` in types; the database handles `halfvec` conversion.
     - Test vector operations with small datasets first to verify index and function behavior.
   - **EAV Structure for Multi-Model Embeddings (EAV结构支持多模型嵌入)**:
     - Use Entity-Attribute-Value pattern for flexible embedding storage across different models:
       ```sql
       CREATE TABLE embeddings (
         id uuid PRIMARY KEY DEFAULT gen_random_uuid(),
         entity_id uuid NOT NULL,              -- Reference to the source record / 源记录引用
         entity_type text NOT NULL,            -- Table/type name (e.g., 'documents', 'images') / 实体类型
         model_name text NOT NULL,             -- Embedding model (e.g., 'openai-3-small', 'clip-vit') / 模型名称
         dimensions int NOT NULL,              -- Vector dimensions for validation / 向量维度
         embedding halfvec NOT NULL,           -- The actual embedding vector / 实际嵌入向量
         created_at timestamptz DEFAULT now(),
         meta jsonb DEFAULT '{}'::jsonb,       -- Model version, parameters, etc. / 模型版本、参数等
         UNIQUE(entity_id, entity_type, model_name)
       );
       CREATE INDEX idx_embeddings_lookup ON embeddings(entity_id, entity_type, model_name);
       ```
     - Benefits: Store embeddings from multiple models (OpenAI, Cohere, CLIP, etc.) for the same entity without schema changes.
     - Create model-specific indexes for optimized similarity search:
       ```sql
       CREATE INDEX idx_emb_openai ON embeddings USING hnsw (embedding halfvec_cosine_ops)
         WHERE model_name = 'text-embedding-4';
       ```
     - Query pattern: Filter by `model_name` first, then perform similarity search for efficient index usage.
     - Consider partitioning by `model_name` for very large datasets to improve query performance.
     - **Helper Function for Model Registration (模型注册辅助函数)**:
       ```sql
       -- Creates HNSW index for a new embedding model / 为新嵌入模型创建HNSW索引
       -- Usage: SELECT create_embedding_model_index('text-embedding-4', 'cosine');
       CREATE OR REPLACE FUNCTION create_embedding_model_index(
         p_model_name text,
         p_distance_metric text DEFAULT 'cosine'  -- 'cosine', 'l2', or 'ip'
       ) RETURNS void AS $$
       DECLARE
         v_index_name text;
         v_ops_class text;
       BEGIN
         -- Generate safe index name / 生成安全的索引名称
         v_index_name := 'idx_emb_' || regexp_replace(p_model_name, '[^a-zA-Z0-9]', '_', 'g');
         
         -- Map distance metric to operator class / 映射距离度量到操作符类
         v_ops_class := CASE p_distance_metric
           WHEN 'cosine' THEN 'halfvec_cosine_ops'
           WHEN 'l2' THEN 'halfvec_l2_ops'
           WHEN 'ip' THEN 'halfvec_ip_ops'
           ELSE 'halfvec_cosine_ops'
         END;
         
         -- Create partial HNSW index for this model / 为此模型创建部分HNSW索引
         EXECUTE format(
           'CREATE INDEX IF NOT EXISTS %I ON embeddings USING hnsw (embedding %s) WHERE model_name = %L',
           v_index_name, v_ops_class, p_model_name
         );
         
         RAISE NOTICE 'Created index % for model % with % distance', v_index_name, p_model_name, p_distance_metric;
       END;
       $$ LANGUAGE plpgsql;
       
       COMMENT ON FUNCTION create_embedding_model_index IS 
         'Creates a partial HNSW index for a specific embedding model. / 为特定嵌入模型创建部分HNSW索引。';
       ```
     - Call this function when introducing a new embedding model to ensure optimized similarity search performance.