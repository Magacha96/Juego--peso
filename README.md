# Juego--peso
Peso
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>¿Qué pesa más? – Nivel Fácil</title>
  <style>
    body { font-family: Arial, sans-serif; text-align: center; margin-top: 30px; }
    .balanza { width: 300px; height: 200px; margin: 0 auto; background: url('https://i.imgur.com/3ZLxB7g.png') no-repeat bottom center; background-size: contain; position: relative; }
    .lado { width: 130px; height: 10px; position: absolute; bottom: 20px; }
    .izq { left: 30px; }
    .der { right: 30px; }
    .obj { width: 60px; cursor: grab; }
    #mensaje { margin-top: 20px; font-size: 1.2em; }
  </style>
</head>
<body>
  <h1>¿Qué pesa más?</h1>
  <p>Arrastra dos objetos a la balanza:</p>
  <div class="balanza">
    <div class="lado izq" ondrop="soltar(event, 'izq')" ondragover="event.preventDefault()"></div>
    <div class="lado der" ondrop="soltar(event, 'der')" ondragover="event.preventDefault()"></div>
  </div>

  <div style="margin-top: 20px;">
    <img src="https://i.imgur.com/0KXqgYQ.png" alt="Pluma" draggable="true" ondragstart="inicioArrastre(event)" data-peso="1" class="obj">
    <img src="https://i.imgur.com/uM8C3C8.png" alt="Libro" draggable="true" ondragstart="inicioArrastre(event)" data-peso="5" class="obj">
    <img src="https://i.imgur.com/2Dt7X5d.png" alt="Globo" draggable="true" ondragstart="inicioArrastre(event)" data-peso="1" class="obj">
    <img src="https://i.imgur.com/5Hp3jDS.png" alt="Silla" draggable="true" ondragstart="inicioArrastre(event)" data-peso="8" class="obj">
  </div>

  <audio id="acierto"><source src="https://actions.google.com/sounds/v1/cartoon/clang_and_wobble.ogg" type="audio/ogg"></audio>
  <audio id="fallo"><source src="https://actions.google.com/sounds/v1/cartoon/boing.ogg" type="audio/ogg"></audio>

  <div id="mensaje"></div>

  <script>
    let arrastrado = null;

    function inicioArrastre(ev) {
      arrastrado = ev.target;
    }

    function soltar(ev, lado) {
      ev.preventDefault();
      if (!arrastrado) return;
      const cont = document.querySelector('.' + lado);
      if (cont.children.length >= 1) return; // solo un objeto por lado
      cont.appendChild(arrastrado);
      chequear();
      arrastrado = null;
    }

    function chequear() {
      const izq = document.querySelector('.izq').firstChild;
      const der = document.querySelector('.der').firstChild;
      const msg = document.getElementById('mensaje');
      if (izq && der) {
        const pesoI = +izq.getAttribute('data-peso');
        const pesoD = +der.getAttribute('data-peso');
        const audio = pesoI !== pesoD 
          ? (pesoI > pesoD ? 'acierto' : 'fallo') 
          : 'fallo';
        document.getElementById(audio).play();
        msg.textContent = pesoI === pesoD ? '¡Iguales!' : pesoI > pesoD ? '¡Muy bien! Izquierda más pesada.' : '¡Muy bien! Derecha más pesada.';
        msg.style.color = audio==='acierto' ? 'green' : 'orange';
      }
    }
  </script>
</body>
</html>
