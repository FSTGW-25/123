<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Love Curve Wave</title>
  <style>
    body {
      margin: 0;
      background: #000;
      color: white;
      font-family: sans-serif;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      height: 100vh;
    }

    canvas {
      border: 2px solid white;
      background: black;
      margin-top: 10px;
    }

    .controls {
      margin-bottom: 10px;
    }

    label, select {
      font-size: 1rem;
    }

    select {
      padding: 4px 8px;
      border-radius: 6px;
      background: #222;
      color: white;
      border: 1px solid #555;
    }
  </style>
</head>
<body>
  <div class="controls">
    <label for="colorPicker">Pilih warna love: </label>
    <select id="colorPicker">
      <option value="#ff4d88">Pink</option>
      <option value="#ff0000">Merah</option>
      <option value="#ffffff">Putih</option>
      <option value="#ffff00">Kuning</option>
      <option value="#00ffff">Cyan</option>
      <option value="#a78bfa">Lilac</option>
      <option value="#00ff00">Hijau</option>
      <option value="#ff00ff">Magenta</option>
    </select>
  </div>

  <canvas id="loveCanvas" width="800" height="600"></canvas>

  <script>
    const canvas = document.getElementById("loveCanvas");
    const ctx = canvas.getContext("2d");
    const width = canvas.width;
    const height = canvas.height;
    const centerX = width / 2;
    const centerY = height / 2;

    const colorPicker = document.getElementById("colorPicker");
    let strokeColor = colorPicker.value;

    const xData = [];
    for (let i = 0; i < 200; i++) {
      xData.push(-2.5 + i * (5 / 199));
    }

    let frame = 0;

    function f(x, k) {
      const sqrtTerm = Math.sqrt(Math.max(0, 3 - x * x));
      return (
        Math.pow(Math.abs(x), 2 / 3) +
        0.9 * sqrtTerm * Math.sin(10 * (k * x - 2 * Math.sin(k))) -
        1.5
      );
    }

    function draw() {
      ctx.clearRect(0, 0, width, height);
      ctx.beginPath();
      ctx.strokeStyle = strokeColor;
      ctx.lineWidth = 2;

      // Lambatkan gerakan dengan kelipatan frame lebih kecil
      const k = -Math.PI + (frame % 300) * (2 * Math.PI / 300);

      xData.forEach((x, i) => {
        const y = f(x - Math.sin(k), k);
        const px = centerX + x * 80;
        const py = centerY - y * 80;

        if (i === 0) {
          ctx.moveTo(px, py);
        } else {
          ctx.lineTo(px, py);
        }
      });

      ctx.stroke();
      frame++;
      requestAnimationFrame(draw);
    }

    // Update warna ketika user memilih
    colorPicker.addEventListener("change", () => {
      strokeColor = colorPicker.value;
    });

    draw();
  </script>
</body>
</html>
