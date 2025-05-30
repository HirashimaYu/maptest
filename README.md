
<!DOCTYPE html>
<html>
  <head>
    <title>マップに地点表示</title>
    <style>
      #map {
        height: 100vh;
        width: 100%;
      }
    </style>
    <script>
      async function initMap() {
        const map = new google.maps.Map(document.getElementById("map"), {
          zoom: 10,
          center: { lat: 35.6762, lng: 139.6503 }, // 東京中心
        });

        // JSONデータ取得
        const response = await fetch("https://script.google.com/macros/s/https://script.google.com/macros/s/AKfycbz8aQQnwtK6X_lNOFXSLMuZz46e_U9gcKdDwbscUbfItYcdr5VAU1mEyttQnr4aDD1HHg/exec/exec");
        const data = await response.json();

        // 各地点をマップに表示
        data.forEach(place => {
          new google.maps.Marker({
            position: { lat: place.lat, lng: place.lng },
            map,
            title: place.name,
          });
        });
      }
    </script>
    <script async defer
      src="https://maps.googleapis.com/maps/api/js?key=AIzaSyClN0LeQjAs9IYscEvrjqzT29G1efRkJK0&callback=initMap">
    </script>
  </head>
  <body>
    <div id="map"></div>
  </body>
</html>
