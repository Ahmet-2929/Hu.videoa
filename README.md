# Hu.videoa
Yeni İslâmi Eğitici Video Yükleme Düzeneği
<!DOCTYPE html>
<html lang="tr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Benim Video Platformum</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 0;
      padding: 0;
      background: #f2f2f2;
    }
    header {
      background: #333;
      color: white;
      padding: 15px;
      text-align: center;
    }
    header h1 {
      margin: 0;
      font-size: 20px;
    }
    nav {
      display: flex;
      justify-content: center;
      gap: 10px;
      margin: 10px;
    }
    nav button {
      padding: 10px;
      border: none;
      border-radius: 5px;
      background: #444;
      color: white;
      cursor: pointer;
    }
    nav button:hover {
      background: #666;
    }
    #videoList {
      padding: 15px;
    }
    .videoCard {
      background: white;
      padding: 10px;
      margin-bottom: 15px;
      border-radius: 10px;
      box-shadow: 0 2px 5px rgba(0,0,0,0.2);
    }
    .videoCard h3 {
      margin: 5px 0;
    }
    form {
      background: white;
      padding: 15px;
      margin: 15px;
      border-radius: 10px;
      box-shadow: 0 2px 5px rgba(0,0,0,0.2);
    }
    form input, form select, form button {
      display: block;
      width: 100%;
      margin: 10px 0;
      padding: 10px;
      border-radius: 5px;
      border: 1px solid #ccc;
    }
    form button {
      background: #333;
      color: white;
      cursor: pointer;
    }
    form button:hover {
      background: #555;
    }
  </style>
</head>
<body>

  <header>
    <h1>🎥 Benim Video Platformum</h1>
  </header>

  <nav>
    <button onclick="renderVideos('Tümü')">Tümü</button>
    <button onclick="renderVideos('Eğitim')">Eğitim</button>
    <button onclick="renderVideos('İslami')">İslami</button>
    <button onclick="renderVideos('Oyun')">Oyun</button>
  </nav>

  <div id="videoList"></div>

  <form>
    <h2>Admin Paneli - Video Ekle</h2>
    <input type="text" placeholder="Video Başlığı" required>
    <input type="text" placeholder="YouTube URL" required>
    <select>
      <option value="Eğitim">Eğitim</option>
      <option value="İslami">İslami</option>
      <option value="Oyun">Oyun</option>
    </select>
    <button type="submit">Videoyu Ekle</button>
  </form>

  <script>
    const videoList = document.getElementById("videoList");
    const form = document.querySelector("form");

    // Videoları kaydetmek için localStorage kullanıyoruz
    let videos = JSON.parse(localStorage.getItem("videos")) || [];

    function renderVideos(filter = "Tümü") {
      videoList.innerHTML = "";
      videos
        .filter(v => filter === "Tümü" || v.category === filter)
        .forEach(v => {
          const div = document.createElement("div");
          div.classList.add("videoCard");

          // YouTube URL’den video ID’sini çıkar
          let videoId = "";
          if (v.url.includes("v=")) {
            videoId = v.url.split("v=")[1];
            const ampersand = videoId.indexOf("&");
            if (ampersand !== -1) {
              videoId = videoId.substring(0, ampersand);
            }
          } else if (v.url.includes("youtu.be/")) {
            videoId = v.url.split("youtu.be/")[1];
          }

          div.innerHTML = `
            <h3>${v.title} (${v.category})</h3>
            <iframe width="100%" height="200" src="https://www.youtube.com/embed/${videoId}" frameborder="0" allowfullscreen></iframe>
          `;
          videoList.appendChild(div);
        });
    }

    form.addEventListener("submit", e => {
      e.preventDefault();
      const title = form.querySelector("input[placeholder='Video Başlığı']").value;
      const url = form.querySelector("input[placeholder='YouTube URL']").value;
      const category = form.querySelector("select").value;

      videos.push({ title, url, category });
      localStorage.setItem("videos", JSON.stringify(videos));
      renderVideos();
      form.reset();
    });

    renderVideos();
  </script>

</body>
</html>
