<!DOCTYPE html>
<html lang="ko">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>수업 타이머 + 활동 안내</title>
    <style>
      body {
        font-family: sans-serif;
        text-align: center;
        padding: 2rem;
        max-width: 600px;
        margin: auto;
      }
      #activity {
        font-size: 1.5rem;
        margin-bottom: 1rem;
      }
      #timer {
        font-size: 2rem;
        margin-bottom: 1rem;
      }
      img {
        max-width: 100%;
        border-radius: 10px;
        margin-top: 1rem;
      }
      .input-block {
        margin-bottom: 1rem;
      }
    </style>
  </head>
  <body>
    <h1>수업 타이머 + 활동 안내</h1>

    <div class="input-block">
      <input type="text" id="titleInput" placeholder="활동 제목 입력" />
    </div>
    <div class="input-block">
      <input type="number" id="durationInput" placeholder="활동 시간(초) 입력" />
    </div>
    <div class="input-block">
      <input type="text" id="imageInput" placeholder="이미지 URL 입력 (선택)" />
    </div>
    <div class="input-block">
      <button onclick="addActivity()">활동 추가</button>
      <button onclick="startActivity(0)">타이머 시작</button>
      <button onclick="resetApp()">초기화</button>
    </div>

    <div id="activity"></div>
    <div id="timer"></div>
    <img id="activityImage" src="" alt="" style="display: none" />

    <script>
      let activities = [];
      let current = 0;
      let timer;

      function addActivity() {
        const title = document.getElementById("titleInput").value.trim();
        const duration = parseInt(document.getElementById("durationInput").value.trim());
        const image = document.getElementById("imageInput").value.trim();

        if (!title || isNaN(duration)) {
          alert("제목과 시간을 정확히 입력해주세요.");
          return;
        }

        activities.push({ title, duration, image });
        document.getElementById("titleInput").value = "";
        document.getElementById("durationInput").value = "";
        document.getElementById("imageInput").value = "";

        alert(`활동 '${title}'이(가) 추가되었습니다.`);
      }

      function startActivity(index) {
        if (index >= activities.length) {
          document.getElementById("activity").textContent = "수업이 종료되었습니다.";
          document.getElementById("timer").textContent = "";
          document.getElementById("activityImage").style.display = "none";
          return;
        }

        current = index;
        const act = activities[index];
        let remaining = act.duration;

        document.getElementById("activity").textContent = act.title;
        document.getElementById("activityImage").src = act.image || "";
        document.getElementById("activityImage").style.display = act.image ? "block" : "none";

        document.getElementById("timer").textContent = formatTime(remaining);

        timer = setInterval(() => {
          remaining--;
          document.getElementById("timer").textContent = formatTime(remaining);
          if (remaining <= 0) {
            clearInterval(timer);
            startActivity(index + 1);
          }
        }, 1000);
      }

      function resetApp() {
        clearInterval(timer);
        activities = [];
        current = 0;
        document.getElementById("activity").textContent = "";
        document.getElementById("timer").textContent = "";
        document.getElementById("activityImage").style.display = "none";
        document.getElementById("titleInput").value = "";
        document.getElementById("durationInput").value = "";
        document.getElementById("imageInput").value = "";
        alert("초기화되었습니다.");
      }

      function formatTime(seconds) {
        const min = Math.floor(seconds / 60);
        const sec = seconds % 60;
        return `${min}:${sec.toString().padStart(2, '0')}`;
      }
    </script>
  </body>
</html>
