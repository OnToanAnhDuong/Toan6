<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Welcome to the Traditional Room</title>
    <script src="https://aframe.io/releases/1.2.0/aframe.min.js"></script>
  </head>
  <body>
    <a-scene>
      <!-- Tạo tường phòng -->
      <!-- Tường phía trước -->
      <a-box position="0 3 -10" depth="1" height="6" width="12" material="color: #F5F5DC"></a-box>

      <!-- Tường bên trái -->
      <a-box position="-6 3 0" depth="1" height="6" width="12" material="color: #2196F3"></a-box>
      
      <!-- Tường bên phải -->
      <a-box position="6 3 0" depth="1" height="6" width="12" material="color: #2196F3"></a-box>

      <!-- Tạo trần nhà -->
      <a-box position="0 6 -10" depth="12" height="1" width="12" material="color: #E0E0E0"></a-box>

      <!-- Tạo sàn phòng -->
      <a-box position="0 0 -10" depth="12" height="0.1" width="12" material="color: #D3D3D3"></a-box>

      <!-- Dòng chữ chào mừng -->
      <a-text value="Welcome to the Traditional Room of Ly Tu Trong Secondary School" 
              position="0 5 -11"  
              color="black" 
              align="center" 
              width="10" 
              scale="3 3 1"></a-text>

      <!-- Tạo khung video bên trái -->
      <a-plane position="-6 2 -8" rotation="0 90 0" width="4" height="2" material="color: #FFEB3B">
        <a-video src="https://www.w3schools.com/html/mov_bbb.mp4" width="4" height="2"></a-video>
      </a-plane>

      <!-- Tạo khung video bên phải -->
      <a-plane position="6 2 -8" rotation="0 -90 0" width="4" height="2" material="color: #FFEB3B">
        <a-video src="https://www.w3schools.com/html/mov_bbb.mp4" width="4" height="2"></a-video>
      </a-plane>

      <!-- Ánh sáng mạnh hơn trong không gian -->
      <a-light type="directional" intensity="2" position="0 5 -5" target="#scene"></a-light>

      <!-- Camera di chuyển trong phòng -->
      <a-camera position="0 1.6 0" wasd-controls="enabled: true"></a-camera>

      <!-- Điều chỉnh màu nền của toàn cảnh -->
      <a-sky color="#81D4FA"></a-sky> <!-- Màu nền xanh sáng để tạo sự nổi bật -->
    </a-scene>
  </body>
</html>
