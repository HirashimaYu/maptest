<!DOCTYPE html>
<html>
  <head>
    <title>マップに地点表示</title>
    <style>
      #map {
        height: 100vh;
        width: 80%;
        float: left;
      }
      #sidebar {
        width: 20%;
        height: 100vh;
        float: left;
        overflow-y: scroll;
        padding: 10px;
        box-sizing: border-box;
        background: #f0f0f0;
      }
      .info-item {
        margin-bottom: 15px;
        border-bottom: 1px solid #ccc;
        padding-bottom: 10px;
      }
    </style>
  </head>
  <body>
    <div id="sidebar"></div>
    <div id="map"></div>

    <script>
      // 関数をグローバルに明示的に定義
      window.initMap = async function () {
        const map = new google.maps.Map(document.getElementById("map"), {
          zoom: 10,
          center: { lat: 35.6762, lng: 139.6503 },
          gestureHandling: "greedy"
        });

        const response = await fetch("https://script.google.com/macros/s/AKfycbwPQ-AxnUMEwcrQfR4Szd9qLKZ8oVNzCnZfElkeB8Ogd_gbAZdaRrjJnbKEq3h2HZuaSw/exec");
        const data = await response.json();

        const infoWindow = new google.maps.InfoWindow();
        const sidebar = document.getElementById("sidebar");

        data.forEach(place => {
          const marker = new google.maps.marker.AdvancedMarkerElement({
            position: { lat: place.lat, lng: place.lng },
            map,
            title: place["行った現場の名前を教えてください"]
          });

          const content = `
            <div>
              <strong>${place["行った現場の名前を教えてください"]}</strong><br>
              <strong>${place.info || "情報なし"}</strong>
            </div>
          `;

          marker.addListener("mouseover", () => {
            infoWindow.setContent(content);
            infoWindow.open(map, marker);
          });

          marker.addListener("mouseout", () => {
            infoWindow.close();
          });

          // 不要な ${place[place]} を削除
          const div = document.createElement("div");
          div.className = "info-item";
          div.innerHTML = `<strong>${place["行った現場の名前を教えてください"]}</strong><br>
            ${place.info || "情報なし"}<br>`;
          sidebar.appendChild(div);
        });
      };
    </script>

    <!-- ライブラリ指定とcallbackを指定 -->
    <script
      src="https://maps.googleapis.com/maps/api/js?key=AIzaSyClN0LeQjAs9IYscEvrjqzT29G1efRkJK0&callback=initMap&libraries=marker"
      defer
    ></script>
  </body>
</html>
