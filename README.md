# image_processing_rock-paper-scissors
# image_processing_rock-paper-scissors
<html>
  <body>
    <div>Teachable Machine Image Model</div>
<button type="button" onclick="init()">Start</button>
<div id="webcam-container"></div>
<div id="label-container"></div>

<script src="https://cdn.jsdelivr.net/npm/@tensorflow/tfjs@latest/dist/tf.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/@teachablemachine/image@latest/dist/teachablemachine-image.min.js"></script>

<!-- CSS for neat layout with stripes -->
<style>
  body {
      font-family: Arial, sans-serif;
      display: flex;
      justify-content: center;
      align-items: center;
      flex-direction: column;
      min-height: 100vh;
      background-color: #f0f0f5;
      margin: 0;
      padding: 20px;
      position: relative;  /* Ensure stripes position relative to the body */
      overflow: hidden;     /* Prevent overflow from stripe lines */
  }

  div {
      font-size: 24px;
      margin-bottom: 10px;
      text-align: center;
  }

  button {
      background-color: #4CAF50;
      border: none;
      color: white;
      padding: 12px 24px;
      text-align: center;
      text-decoration: none;
      display: inline-block;
      font-size: 18px;
      margin: 4px 2px;
      cursor: pointer;
      border-radius: 8px;
  }

  button:hover {
      background-color: #45a049;
  }

  #webcam-container {
      margin-top: 20px;
      border: 2px solid #ddd;
      border-radius: 10px;
      width: 220px;
      height: 220px;
      display: flex;
      justify-content: center;
      align-items: center;
      background-color: #fff;
  }

  #label-container div {
      margin-top: 10px;
      font-size: 18px;
      color: #333;
      padding: 8px;
      background-color: #e0e0eb;
      border-radius: 5px;
      width: fit-content;
      margin-left: auto;
      margin-right: auto;
  }

  #label-container {
      margin-top: 20px;
  }

  /* Stripe lines at top and bottom */
  .stripe {
      position: absolute;
      width: 200%;
      height: 30px;
      background: linear-gradient(135deg, #3498db 25%, transparent 25%, transparent 50%, #3498db 50%, #3498db 75%, transparent 75%);
      background-size: 40px 40px;
  }

  .stripe-top {
      top: 0;
      left: -50%;
  }

  .stripe-bottom {
      bottom: 0;
      left: -50%;
  }

  /* Ensure proper spacing for the content and stripes */
  html, body {
      height: 100%;
      margin: 0;
      padding: 0;
  }
</style>

<!-- Stripe elements for top and bottom -->
<div class="stripe stripe-top"></div>
<div class="stripe stripe-bottom"></div>

<script type="text/javascript">
    const URL = "https://teachablemachine.withgoogle.com/models/JG_Qe3GUB/";

    let model, webcam, labelContainer, maxPredictions;

    async function init() {
        const modelURL = URL + "model.json";
        const metadataURL = URL + "metadata.json";

        model = await tmImage.load(modelURL, metadataURL);
        maxPredictions = model.getTotalClasses();

        const flip = true;
        webcam = new tmImage.Webcam(200, 200, flip);
        await webcam.setup();
        await webcam.play();
        window.requestAnimationFrame(loop);

        document.getElementById("webcam-container").appendChild(webcam.canvas);
        labelContainer = document.getElementById("label-container");
        for (let i = 0; i < maxPredictions; i++) {
            labelContainer.appendChild(document.createElement("div"));
        }
    }

    async function loop() {
        webcam.update();
        await predict();
        window.requestAnimationFrame(loop);
    }

    async function predict() {
        const prediction = await model.predict(webcam.canvas);
        for (let i = 0; i < maxPredictions; i++) {
            const classPrediction = prediction[i].className + ": " + prediction[i].probability.toFixed(2);
            labelContainer.childNodes[i].innerHTML = classPrediction;
        }
    }
</script>
</body>
</html>
