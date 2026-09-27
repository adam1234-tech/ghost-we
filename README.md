# ghost-we
<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>GHOST - Webhook Control Hub 👻</title>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
  <style>
    :root {
      --bg: #090d16;
      --card-bg: rgba(17, 24, 39, 0.75);
      --border: rgba(255, 255, 255, 0.08);
      --primary: #a855f7;
      --primary-glow: rgba(168, 85, 247, 0.35);
      --accent: #6366f1;
      --text: #f8fafc;
      --text-muted: #64748b;
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
    }

    body {
      background-color: var(--bg);
      background-image: 
        radial-gradient(at 10% 10%, rgba(168, 85, 247, 0.12) 0px, transparent 50%),
        radial-gradient(at 90% 90%, rgba(99, 102, 241, 0.12) 0px, transparent 50%);
      color: var(--text);
      min-height: 100vh;
      display: flex;
      flex-direction: column;
    }

    header {
      padding: 20px 40px;
      border-bottom: 1px solid var(--border);
      display: flex;
      justify-content: space-between;
      align-items: center;
      backdrop-filter: blur(10px);
      background: rgba(9, 13, 22, 0.85);
      position: sticky;
      top: 0;
      z-index: 100;
    }

    .logo {
      font-size: 24px;
      font-weight: 800;
      letter-spacing: 1px;
      background: linear-gradient(135deg, var(--primary), var(--accent));
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      display: flex;
      align-items: center;
      gap: 12px;
    }

    .container {
      display: grid;
      grid-template-columns: 320px 1fr 380px;
      gap: 25px;
      padding: 30px 40px;
      flex: 1;
      max-width: 1750px;
      margin: 0 auto;
      width: 100%;
    }

    @media (max-width: 1200px) {
      .container { grid-template-columns: 1fr; }
    }

    .panel {
      background: var(--card-bg);
      border: 1px solid var(--border);
      border-radius: 16px;
      padding: 24px;
      backdrop-filter: blur(16px);
      box-shadow: 0 20px 40px rgba(0, 0, 0, 0.5);
      display: flex;
      flex-direction: column;
      gap: 20px;
    }

    .panel-title {
      font-size: 16px;
      font-weight: 700;
      color: var(--primary);
      display: flex;
      align-items: center;
      gap: 8px;
      border-bottom: 1px solid var(--border);
      padding-bottom: 12px;
    }

    .form-group {
      display: flex;
      flex-direction: column;
      gap: 8px;
    }

    label {
      font-size: 13px;
      color: var(--text-muted);
      font-weight: 600;
    }

    input, textarea, select {
      background: rgba(15, 23, 42, 0.6);
      border: 1px solid var(--border);
      border-radius: 10px;
      padding: 12px 14px;
      color: #fff;
      font-size: 14px;
      outline: none;
      transition: all 0.2s ease;
    }

    input:focus, textarea:focus, select:focus {
      border-color: var(--primary);
      box-shadow: 0 0 15px var(--primary-glow);
    }

    textarea {
      resize: vertical;
      min-height: 110px;
    }

    .row {
      display: flex;
      gap: 12px;
    }

    .row > * { flex: 1; }

    /* Saved Webhooks List */
    .webhook-item {
      background: rgba(255, 255, 255, 0.03);
      border: 1px solid var(--border);
      padding: 12px 14px;
      border-radius: 12px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      cursor: pointer;
      transition: all 0.2s ease;
    }

    .webhook-item:hover {
      background: rgba(168, 85, 247, 0.12);
      border-color: var(--primary);
      transform: translateX(-3px);
    }

    .webhook-actions {
      display: flex;
      gap: 8px;
    }

    .action-btn {
      background: rgba(255, 255, 255, 0.05);
      border: 1px solid var(--border);
      color: var(--text-muted);
      width: 32px;
      height: 32px;
      border-radius: 8px;
      display: flex;
      align-items: center;
      justify-content: center;
      cursor: pointer;
      transition: 0.2s;
    }

    .action-btn:hover {
      color: #fff;
      background: rgba(255, 255, 255, 0.15);
    }

    .action-btn.edit:hover { color: var(--primary); border-color: var(--primary); }
    .action-btn.delete:hover { color: #ef4444; border-color: #ef4444; }

    .btn {
      padding: 12px 20px;
      border-radius: 10px;
      font-weight: 700;
      border: none;
      cursor: pointer;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 8px;
      transition: all 0.2s ease;
    }

    .btn-primary {
      background: linear-gradient(135deg, #a855f7, #6366f1);
      color: #fff;
      box-shadow: 0 4px 20px rgba(168, 85, 247, 0.4);
    }

    .btn-primary:hover {
      transform: translateY(-2px);
      box-shadow: 0 6px 25px rgba(168, 85, 247, 0.6);
    }

    .btn-secondary {
      background: rgba(255, 255, 255, 0.05);
      border: 1px solid var(--border);
      color: #fff;
    }

    .btn-secondary:hover {
      background: rgba(255, 255, 255, 0.1);
    }

    /* Discord Live Preview */
    .discord-preview {
      background: #313338;
      border-radius: 12px;
      padding: 16px;
      display: flex;
      gap: 16px;
    }

    .discord-avatar {
      width: 40px;
      height: 40px;
      border-radius: 50%;
      background: #5865f2;
      object-fit: cover;
    }

    .discord-body {
      flex: 1;
    }

    .discord-author {
      font-weight: 600;
      font-size: 15px;
      color: #f2f3f5;
      display: flex;
      align-items: center;
      gap: 8px;
    }

    .bot-tag {
      background: #5865f2;
      font-size: 10px;
      padding: 2px 4px;
      border-radius: 4px;
      font-weight: 700;
    }

    .discord-text {
      color: #dbdee1;
      font-size: 14px;
      margin-top: 6px;
      white-space: pre-wrap;
      word-break: break-word;
    }

    .discord-embed {
      margin-top: 8px;
      background: #2b2d31;
      border-right: 4px solid var(--primary);
      border-radius: 4px;
      padding: 12px;
    }

    .discord-embed-title {
      font-weight: 700;
      color: #fff;
      margin-bottom: 6px;
    }

    #status {
      padding: 12px;
      border-radius: 8px;
      text-align: center;
      font-size: 14px;
      display: none;
    }

    .status-success { background: rgba(16, 185, 129, 0.2); color: #34d399; border: 1px solid #10b981; }
    .status-error { background: rgba(239, 68, 68, 0.2); color: #f87171; border: 1px solid #ef4444; }

    /* Edit Modal Overlay */
    .modal-overlay {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(0, 0, 0, 0.7);
      backdrop-filter: blur(8px);
      display: none;
      justify-content: center;
      align-items: center;
      z-index: 1000;
    }

    .modal {
      background: #111827;
      border: 1px solid var(--border);
      border-radius: 16px;
      padding: 24px;
      width: 100%;
      max-width: 450px;
      display: flex;
      flex-direction: column;
      gap: 18px;
      box-shadow: 0 25px 50px rgba(0, 0, 0, 0.8);
    }

    .modal-title {
      font-size: 18px;
      font-weight: 700;
      color: var(--primary);
      display: flex;
      align-items: center;
      gap: 10px;
    }
  </style>
</head>
<body>

  <header>
    <div class="logo"><i class="fa-solid fa-ghost"></i> GHOST WEBHOOK HUB</div>
    <div style="color: var(--text-muted); font-size: 14px;"><i class="fa-solid fa-signal" style="color: #10b981;"></i> GHOST Engine: Active</div>
  </header>

  <div class="container">
    
    <!-- Panel 1: Saved Webhooks -->
    <div class="panel">
      <div class="panel-title"><i class="fa-solid fa-bookmark"></i> قائمة GHOST Webhooks</div>
      
      <div class="form-group">
        <label>تسمية الـ Webhook الجديد:</label>
        <input type="text" id="saveName" placeholder="مثلاً: GHOST RP Logs">
      </div>
      <button class="btn btn-secondary" onclick="saveCurrentWebhook()"><i class="fa-solid fa-plus"></i> حفظ الـ URL الحالي</button>

      <div style="display: flex; flex-direction: column; gap: 10px; margin-top: 10px;" id="webhooksList">
        <!-- Dynamic list -->
      </div>
    </div>

    <!-- Panel 2: Message Builder -->
    <div class="panel">
      <div class="panel-title"><i class="fa-solid fa-pen-to-square"></i> محرر الرسائل (GHOST Builder)</div>

      <div class="form-group">
        <label>رابط الـ Webhook (URL):</label>
        <input type="url" id="webhookUrl" placeholder="https://discord.com/api/webhooks/..." oninput="updatePreview()">
      </div>

      <div class="row">
        <div class="form-group">
          <label>نوع المسج:</label>
          <select id="msgMode" onchange="toggleMode(); updatePreview();">
            <option value="embed">Embed Card (احترافي)</option>
            <option value="normal">Normal Text (عادي)</option>
          </select>
        </div>
        <div class="form-group" id="colorGroup">
          <label>لون الكارت:</label>
          <input type="color" id="embedColor" value="#a855f7" style="height: 44px; padding: 2px; cursor: pointer;" oninput="updatePreview()">
        </div>
      </div>

      <div class="form-group" id="titleGroup">
        <label>العنوان (Title):</label>
        <input type="text" id="embedTitle" placeholder="مثلاً: 🚨 GHOST System Alert" oninput="updatePreview()">
      </div>

      <div class="form-group">
        <label>المحتوى (Message Body):</label>
        <textarea id="msgBody" placeholder="كتب المسج ديالك هنا..." oninput="updatePreview()">سلام! هادا مسج مصيفط من GHOST Webhook Hub 👻</textarea>
      </div>

      <!-- Presets -->
      <div class="form-group">
        <label>نماذج سريعة (GHOST Presets):</label>
        <div class="row">
          <button class="btn btn-secondary" style="padding: 6px;" onclick="applyPreset('info')">Info ℹ️</button>
          <button class="btn btn-secondary" style="padding: 6px;" onclick="applyPreset('success')">Success ✅</button>
          <button class="btn btn-secondary" style="padding: 6px;" onclick="applyPreset('alert')">Alert 🚨</button>
        </div>
      </div>

      <button class="btn btn-primary" onclick="sendWebhook()"><i class="fa-solid fa-paper-plane"></i> إرسال الرسالة الآن</button>
      <div id="status"></div>
    </div>

    <!-- Panel 3: Live Preview -->
    <div class="panel">
      <div class="panel-title"><i class="fa-solid fa-eye"></i> معاينة مباشرة (Live Preview)</div>
      
      <div class="discord-preview">
        <img src="https://cdn.discordapp.com/embed/avatars/0.png" class="discord-avatar" alt="Avatar">
        <div class="discord-body">
          <div class="discord-author">
            <span>Webhook (Original Name)</span>
            <span class="bot-tag">BOT</span>
          </div>
          
          <div id="prevNormalText" class="discord-text" style="display: none;"></div>

          <div id="prevEmbed" class="discord-embed">
            <div id="prevEmbedTitle" class="discord-embed-title">🚨 GHOST System Alert</div>
            <div id="prevEmbedBody" class="discord-text">سلام! هادا مسج مصيفط من GHOST Webhook Hub 👻</div>
          </div>
        </div>
      </div>
    </div>

  </div>

  <!-- Edit Modal -->
  <div class="modal-overlay" id="editModal">
    <div class="modal">
      <div class="modal-title"><i class="fa-solid fa-pen"></i> تعديل تسمية الـ Webhook</div>
      <div class="form-group">
        <label>الاسم الجديد:</label>
        <input type="text" id="editNameInput">
      </div>
      <div class="form-group">
        <label>الرابط (URL):</label>
        <input type="url" id="editUrlInput">
      </div>
      <div class="row">
        <button class="btn btn-secondary" onclick="closeEditModal()">إلغاء</button>
        <button class="btn btn-primary" onclick="saveEditedWebhook()">حفظ التعديلات 💾</button>
      </div>
    </div>
  </div>

  <script>
    let savedWebhooks = JSON.parse(localStorage.getItem('ghost_webhooks') || '[]');
    let currentEditIndex = null;

    function renderSavedList() {
      const container = document.getElementById('webhooksList');
      container.innerHTML = '';
      if(savedWebhooks.length === 0) {
        container.innerHTML = `<div style="color:var(--text-muted); font-size:13px; text-align:center; padding:10px;">لا توجد روابط محفوظة بعد.</div>`;
        return;
      }
      
      savedWebhooks.forEach((item, index) => {
        container.innerHTML += `
          <div class="webhook-item" onclick="loadWebhook('${item.url}')">
            <div style="overflow: hidden; padding-left: 8px;">
              <div style="font-weight:700; font-size: 14px; white-space: nowrap; overflow: hidden; text-overflow: ellipsis;">${item.name}</div>
              <div style="font-size:11px; color:var(--text-muted); white-space: nowrap; overflow: hidden; text-overflow: ellipsis;">${item.url}</div>
            </div>
            <div class="webhook-actions">
              <button class="action-btn edit" title="تعديل الاسم" onclick="event.stopPropagation(); openEditModal(${index})"><i class="fa-solid fa-pen"></i></button>
              <button class="action-btn delete" title="حذف" onclick="event.stopPropagation(); deleteWebhook(${index})"><i class="fa-solid fa-trash"></i></button>
            </div>
          </div>
        `;
      });
    }

    function saveCurrentWebhook() {
      const url = document.getElementById('webhookUrl').value.trim();
      const name = document.getElementById('saveName').value.trim() || 'GHOST Webhook ' + (savedWebhooks.length + 1);
      if (!url) return alert('حط رابط Webhook الأول!');
      
      savedWebhooks.push({ name, url });
      localStorage.setItem('ghost_webhooks', JSON.stringify(savedWebhooks));
      document.getElementById('saveName').value = '';
      renderSavedList();
    }

    function openEditModal(index) {
      currentEditIndex = index;
      document.getElementById('editNameInput').value = savedWebhooks[index].name;
      document.getElementById('editUrlInput').value = savedWebhooks[index].url;
      document.getElementById('editModal').style.display = 'flex';
    }

    function closeEditModal() {
      document.getElementById('editModal').style.display = 'none';
      currentEditIndex = null;
    }

    function saveEditedWebhook() {
      if (currentEditIndex !== null) {
        const newName = document.getElementById('editNameInput').value.trim();
        const newUrl = document.getElementById('editUrlInput').value.trim();
        
        if (!newName || !newUrl) return alert('عمر السمية والـ URL بليز!');
        
        savedWebhooks[currentEditIndex] = { name: newName, url: newUrl };
        localStorage.setItem('ghost_webhooks', JSON.stringify(savedWebhooks));
        renderSavedList();
        closeEditModal();
      }
    }

    function deleteWebhook(index) {
      savedWebhooks.splice(index, 1);
      localStorage.setItem('ghost_webhooks', JSON.stringify(savedWebhooks));
      renderSavedList();
    }

    function loadWebhook(url) {
      document.getElementById('webhookUrl').value = url;
      updatePreview();
    }

    function toggleMode() {
      const mode = document.getElementById('msgMode').value;
      const isEmbed = mode === 'embed';
      document.getElementById('colorGroup').style.display = isEmbed ? 'flex' : 'none';
      document.getElementById('titleGroup').style.display = isEmbed ? 'flex' : 'none';
    }

    function updatePreview() {
      const mode = document.getElementById('msgMode').value;
      const body = document.getElementById('msgBody').value || '...';
      const title = document.getElementById('embedTitle').value;
      const color = document.getElementById('embedColor').value;

      const embedBox = document.getElementById('prevEmbed');
      const normalText = document.getElementById('prevNormalText');

      if (mode === 'embed') {
        embedBox.style.display = 'block';
        normalText.style.display = 'none';
        embedBox.style.borderRightColor = color;
        document.getElementById('prevEmbedTitle').innerText = title;
        document.getElementById('prevEmbedTitle').style.display = title ? 'block' : 'none';
        document.getElementById('prevEmbedBody').innerText = body;
      } else {
        embedBox.style.display = 'none';
        normalText.style.display = 'block';
        normalText.innerText = body;
      }
    }

    function applyPreset(type) {
      document.getElementById('msgMode').value = 'embed';
      toggleMode();
      if (type === 'info') {
        document.getElementById('embedTitle').value = 'ℹ️ GHOST Info Update';
        document.getElementById('embedColor').value = '#a855f7';
      } else if (type === 'success') {
        document.getElementById('embedTitle').value = '✅ GHOST Action Successful';
        document.getElementById('embedColor').value = '#10b981';
      } else if (type === 'alert') {
        document.getElementById('embedTitle').value = '🚨 GHOST System Alert';
        document.getElementById('embedColor').value = '#ef4444';
      }
      updatePreview();
    }

    async function sendWebhook() {
      const url = document.getElementById('webhookUrl').value.trim();
      const mode = document.getElementById('msgMode').value;
      const body = document.getElementById('msgBody').value.trim();
      const statusDiv = document.getElementById('status');

      if (!url || !body) {
        statusDiv.className = 'status-error';
        statusDiv.style.display = 'block';
        statusDiv.innerText = 'عمر رابط Webhook والمحتوى بليز!';
        return;
      }

      let payload = {};

      if (mode === 'normal') {
        payload.content = body;
      } else {
        const title = document.getElementById('embedTitle').value.trim();
        const color = parseInt(document.getElementById('embedColor').value.replace('#', ''), 16);
        payload.embeds = [{
          title: title || undefined,
          description: body,
          color: color
        }];
      }

      statusDiv.style.display = 'block';
      statusDiv.className = '';
      statusDiv.innerText = 'جاري الإرسال عبر GHOST Engine... ⏳';

      try {
        const res = await fetch(url, {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify(payload)
        });

        if (res.ok || res.status === 204) {
          statusDiv.className = 'status-success';
          statusDiv.innerText = 'تم إرسال المسج بنجاح عبر GHOST! 👻🚀';
        } else {
          statusDiv.className = 'status-error';
          statusDiv.innerText = `خطأ فـ الإرسال (${res.status})`;
        }
      } catch (err) {
        statusDiv.className = 'status-error';
        statusDiv.innerText = 'فشل الاتصال بـ Webhook API!';
      }
    }

    // Init
    renderSavedList();
    toggleMode();
    updatePreview();
  </script>
</body>
</html>
