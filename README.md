# CUSP 社會處方需求評估（Supabase 免費版）

檔案：index.html（問卷，填答者免登入）、admin.html（後台，需登入）、config.js、supabase.sql、.github/workflows/keepalive.yml

## 部署步驟（約 30 分鐘）
1. **建立 Supabase 專案**：supabase.com 註冊（免費）→ New project。
2. **建立資料表與權限**：左側 SQL Editor → 貼上 `supabase.sql` 全部內容 → Run。
3. **填 config.js**：Project Settings → API，複製 Project URL 與 `anon` public key 填入。⚠️ 不可使用 service_role key。
4. **上線網站**：建立 GitHub repo，上傳全部檔案（含 .github 資料夾）→ Settings → Pages → 從 main 分支發布。問卷網址為 `https://帳號.github.io/repo/`，後台為 `.../admin.html`。
5. **建立第一位擁有者**：開啟 admin.html →「申請帳號」註冊（若 Supabase 要求信箱驗證，先完成驗證）→ 回 SQL Editor 執行：
   `update public.staff set role='owner' where email='你的信箱';`
   之後重新登入。其他人員註冊後，由你在「人員管理」指定角色。
6. **防止專案被暫停**：repo → Settings → Secrets and variables → Actions，新增 `SUPABASE_URL`、`SUPABASE_ANON_KEY`。keepalive 會每 3 天自動請求一次。
7. **填寫隱私說明**：在 index.html 搜尋「請在上線前填入」，換成單位名稱與聯絡方式。

## 多社區分流
問卷網址加 `?site=社區名`，例如 `index.html?site=北投A里`。個案管理師在「人員管理」被設為相同 site，就只看得到該 site 的資料。

## 上線前必測（我無法替你測試）
- 用**未登入**的無痕視窗填問卷，確認後台出現資料。
- 在無痕視窗的瀏覽器主控台執行 `fetch(CUSP_CONFIG.url+"/rest/v1/responses?select=*",{headers:{apikey:CUSP_CONFIG.key}}).then(r=>r.json()).then(console.log)`，**必須回傳空陣列或權限錯誤**，不能看到任何資料。
- 用「待核准」帳號登入，確認看不到資料；用個案管理師帳號確認只看到自己的 site。

## 尚未包含
防機器人驗證（建議加 Cloudflare Turnstile）、送出頻率限制、依追蹤代碼的前後測比較頁、自動備份（免費方案無備份，請定期用後台「下載 CSV」）、個資法與倫理審查文件。
