# hainguyen
<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title> Để lại vài dòng cho tui 💌</title>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      background: #fffaf5;
      color: #333;
      max-width: 600px;
      margin: 30px auto;
      padding: 20px;
    }
    h1 {
      text-align: center;
      color: #d46a6a;
    }
    form {
      background: #ffeaea;
      padding: 15px;
      border-radius: 10px;
      margin-bottom: 20px;
    }
    input, textarea {
      width: 100%;
      margin: 8px 0;
      padding: 10px;
      border-radius: 8px;
      border: 1px solid #ccc;
      font-size: 16px;
    }
    button {
      background-color: #ff8c94;
      color: white;
      border: none;
      padding: 10px 15px;
      font-size: 16px;
      border-radius: 8px;
      cursor: pointer;
    }
    #memories {
      background: #fff0f0;
      padding: 15px;
      border-radius: 10px;
    }
    .memory {
      background: #ffe3e3;
      padding: 10px;
      border-radius: 8px;
      margin-bottom: 10px;
    }
    .memory strong {
      color: #c84b4b;
    }
  </style>
</head>
<body>
  <h1>💌 Gửi cho tui nhé love u <3 !</h1>

  <form id="memoryForm">
    <input type="text" id="name" placeholder="Tên của bạn..." required />
    <textarea id="message" placeholder="Viết lời nhắn/lưu bút..." required></textarea>
    <button type="submit">Gửi lưu bút</button>
  </form>

  <div id="memories">
    <h2>📚 Những dòng lưu bút:</h2>
    <div id="memoryList"></div>
  </div>

  <script>
    const scriptURL = 'https://script.google.com/macros/s/AKfycbxXB4oqG5MI7hY_wKp7LA8DXwjacK6Pu3SRTbgszbzzbsJ1E5pIBOuM5oQaUeXCi9rN/exec'; // ← Dán link Apps Script của bạn vào đây

    const form = document.getElementById('memoryForm');
    const memoryList = document.getElementById('memoryList');

    form.addEventListener('submit', e => {
      e.preventDefault();
      const name = document.getElementById('name').value.trim();
      const message = document.getElementById('message').value.trim();

      if (!name || !message) return;

      fetch(scriptURL, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({ name, message })
      })
      .then(res => res.json())
      .then(() => {
        form.reset();
        loadMessages();
      })
      .catch(error => {
        alert('Có lỗi khi gửi lưu bút!');
        console.error('Error!', error.message);
      });
    });

    function loadMessages() {
      fetch(scriptURL)
        .then(res => res.json())
        .then(data => {
          memoryList.innerHTML = data.reverse().map(item => `
            <div class="memory">
              <strong>${item.name}</strong>: ${item.message}
            </div>
          `).join('');
        });
    }

    window.onload = loadMessages;
  </script>
</body>
</html>
