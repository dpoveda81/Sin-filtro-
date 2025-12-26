<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sin Filtro - El Juego</title>
    <style>
        :root {
            --bg-color: #0d0d0d;
            --card-color: #1a1a1a;
            --accent-color: #e63946;
            --text-color: #f1faee;
        }
        body { background-color: var(--bg-color); color: var(--text-color); font-family: 'Segoe UI', sans-serif; display: flex; flex-direction: column; align-items: center; justify-content: center; height: 100vh; margin: 0; overflow: hidden; }
        .container { text-align: center; width: 90%; max-width: 400px; }
        h1 { font-size: 1.2rem; letter-spacing: 4px; text-transform: uppercase; color: var(--accent-color); margin-bottom: 2rem; }
        #card { background: var(--card-color); height: 350px; border-radius: 20px; display: flex; align-items: center; justify-content: center; padding: 25px; box-shadow: 0 10px 40px rgba(0,0,0,0.7); border: 1px solid #333; margin-bottom: 2rem; transition: transform 0.4s; cursor: pointer; position: relative; }
        #content { font-size: 1.3rem; line-height: 1.4; font-weight: 400; text-shadow: 1px 1px 2px black; }
        button { background: var(--accent-color); color: white; border: none; padding: 15px 35px; border-radius: 50px; font-weight: bold; text-transform: uppercase; letter-spacing: 2px; cursor: pointer; transition: 0.3s; }
        button:active { transform: scale(0.9); }
        .footer { font-size: 0.7rem; color: #555; margin-top: 15px; letter-spacing: 1px; }
    </style>
</head>
<body>
    <div class="container">
        <h1>Sin Filtro</h1>
        <div id="card" onclick="nextCard()">
            <div id="content">¿Estás listo para la verdad?<br><br>Pulsa para empezar.</div>
        </div>
        <button onclick="nextCard()">Siguiente Carta</button>
        <div class="footer" id="counter">Toca la carta o el botón</div>
    </div>

    <script>
        const cards = [
            "Pregunta: ¿Qué es lo que más te asusta de la primera impresión que causas?",
            "Reto: Elige a alguien y dinos qué tres adjetivos 'prohibidos' le pondrías.",
            "Pregunta: ¿Quién de aquí crees que oculta el pasado más turbio?",
            "Pregunta: Si tuviéramos que elegir a alguien de la mesa para un 'crimen pasional', ¿quién sería?",
            "Insinuación: ¿A quién de aquí le confiarías tus llaves, pero no a tu pareja?",
            "Pregunta: ¿Qué rasgo físico mío crees que es mi mayor distracción cuando hablo?",
            "Reto de Tensión: Mantén contacto visual con la persona que más te atraiga por 20 segundos.",
            "Pregunta: ¿Qué es lo que la gente suele malinterpretar de tu lenguaje corporal?",
            "Pregunta: ¿Crees que soy alguien que toma la iniciativa o que espera a ser seducido/a?",
            "Reto: Describe el 'aura' de la persona a tu derecha usando palabras que evoquen sensaciones físicas.",
            "Pregunta: ¿Quién de aquí parece más 'bueno/a' pero sospechas que es el/la más travieso/a?",
            "Pregunta: ¿Qué ropa mía te gustaría que no llevara puesta ahora mismo?",
            "Reto: Imita cómo crees que liga la persona que tienes enfrente.",
            "Pregunta: ¿Qué parte de mi personalidad crees que es puro 'marketing'?",
            "Pregunta: Si pudieras hacerme una pregunta con respuesta de verdad absoluta, ¿cuál sería?",
            "Reto: Dale un cumplido a alguien que sea tan específico que lo haga sonrojar.",
            "Pregunta: ¿A quién de aquí besarías solo por curiosidad, sin que signifique nada?",
            "Pregunta: ¿Qué es lo que más te gusta de que te miren cuando no te das cuenta?",
            "Reto de Confianza: Déjate vendar los ojos y adivina quién de la mesa te toca la mano.",
            "Pregunta: ¿Qué es lo que más te divierte de mi forma de ser 'adulto'?",
            "Pregunta: ¿Qué es lo más egoísta que has hecho por placer?",
            "Reto: Confiesa cuál es el lugar más extraño donde has tenido un encuentro íntimo.",
            "Pregunta: ¿Qué fantasía tienes que te hace sentir vulnerable al contarla?",
            "Pregunta: ¿A qué persona de esta mesa te llevarías a una fiesta secreta?",
            "Insinuación: Si nos quedamos trabados en un ascensor, ¿qué sería lo primero que me dirías?",
            "Pregunta: ¿Qué importancia tiene para ti el olor de alguien en el deseo?",
            "Reto: Susúrrale a alguien una confesión que nunca hayas dicho en público.",
            "Pregunta: ¿Qué es lo que más te cuesta admitir que te gusta en la cama?",
            "Pregunta: ¿Eres de los que prefiere pedir perdón o pedir permiso al conquistar?",
            "Reto: Elige a alguien para que decida qué prenda de ropa debes quitarte o reacomodarte.",
            "Pregunta: ¿Cuál es tu zona más sensible y quién de aquí crees que sabría tratarla mejor?",
            "Pregunta: ¿Qué es lo que más te desenfoca (turn-off) de alguien que te gusta?",
            "Reto: Describe tu outfit favorito para seducir o muestra una foto atrevida de tu móvil.",
            "Pregunta: Si pudieras borrar una experiencia sexual de tu pasado, ¿cuál sería?",
            "Pregunta: ¿Qué te hace sentir más deseado/a: palabras, gestos o silencio?",
            "Reto: Toca el cuello de la persona a tu izquierda y describe la sensación de su piel.",
            "Pregunta: ¿Cuál es el límite que nunca dejarías que alguien cruzara?",
            "Pregunta: ¿Qué es lo que más te atrae de alguien que 'no es tu tipo'?",
            "Reto: Si tuvieras que intercambiar un beso con alguien aquí para salvar tu vida, ¿a quién elegirías?",
            "Pregunta: ¿Qué es lo que más extrañas de tu vida de soltero/a?",
            "Pregunta: ¿Cuál es la mayor mentira que has dicho para acostarte con alguien?",
            "Reto: Cuéntanos tu peor cita de la historia con detalles.",
            "Pregunta: ¿Qué relación te rompió tanto que cambió tu forma de amar?",
            "Pregunta: ¿Qué buscas en los demás que no eres capaz de darte a ti?",
            "Insinuación: ¿Quién de aquí crees que sería el mejor amante y por qué?",
            "Pregunta: ¿Alguna vez has usado a alguien para olvidar a otra persona?",
            "Reto: Llama a alguien 'mi amor' las próximas tres rondas de forma natural.",
            "Pregunta: ¿Qué es lo que más te asusta de que alguien te conozca de verdad?",
            "Pregunta: ¿Qué parte de tu historia te da vergüenza contar, pero te define?",
            "Reto: Di algo que te molesta de la persona enfrente, pero con una sonrisa.",
            "Pregunta: ¿Prefieres dominar o que te dominen en una situación de tensión?",
            "Pregunta: ¿Qué es lo que nunca le has perdonado a un ex?",
            "Reto: Muestra tus últimos 3 chats de WhatsApp o envía un 'te extraño' a alguien.",
            "Pregunta: ¿A quién de aquí le confiarías un secreto que arruinaría tu reputación?",
            "Pregunta: ¿Cuál es el riesgo más grande que has tomado por amor?",
            "Reto: Hazle un masaje de hombros a la persona que más tensa veas en la mesa.",
            "Pregunta: ¿Qué es lo que te hace llorar cuando nadie te ve?",
            "Pregunta: ¿Qué es lo más 'tóxico' que has hecho por celos?",
            "Reto: Confiesa quién de la mesa te ha parecido más atractivo/a hoy.",
            "Pregunta: ¿Qué es lo que más te pesa de tu pasado emocional?",
            "Pregunta: ¿Qué te hace sentir más poderoso/a en una habitación llena de gente?",
            "Reto: Elige a un 'esclavo' en la mesa que deba traerte tu próxima bebida.",
            "Pregunta: Si pudieras leer la mente de alguien aquí por 5 minutos, ¿a quién elegirías?",
            "Pregunta: ¿Qué es lo que más te excita de un desafío intelectual?",
            "Insinuación: Si estuviéramos en una película prohibida, ¿qué papel me darías a mí?",
            "Pregunta: ¿Qué es lo que más te gusta de envejecer?",
            "Reto: Quítate los zapatos y juega el resto de la partida así.",
            "Pregunta: ¿Qué conversación tienes pendiente contigo mismo/a?",
            "Pregunta: ¿Qué harías si supieras que no vas a fracasar en el amor?",
            "Reto: Di algo que te gustaría hacerme si no hubiera nadie más presente.",
            "Pregunta: ¿Cuál es tu mayor placer culposo?",
            "Pregunta: ¿Qué es lo más atrevido que has hecho en un lugar público?",
            "Reto: Muerde suavemente la oreja o cuello de alguien (con su permiso).",
            "Pregunta: ¿Qué parte de ti sientes que nadie ha descubierto del todo?",
            "Pregunta: ¿Crees en la fidelidad eterna o es un concepto pasado de moda?",
            "Reto: Pon tu canción favorita para 'el mood' y baila un poco con alguien.",
            "Pregunta: ¿Qué te hace sentir más vulnerable frente a quien te gusta?",
            "Pregunta: ¿Quién de aquí crees que sería el más divertido en una cama elástica?",
            "Reto: Describe tu noche ideal de pasión usando solo 5 palabras.",
            "Pregunta: ¿Qué es lo que más te asusta de tu propio potencial?",
            "Pregunta: ¿Qué es lo que más te ha hecho dudar de tus gustos?",
            "Reto: Bebe un trago largo si has tenido un pensamiento impuro con alguien de aquí hoy.",
            "Pregunta: ¿Qué es lo que más te hace sentir orgulloso/a de tu cuerpo hoy?",
            "Pregunta: ¿Qué es lo que más te cuesta perdonarte?",
            "Insinuación: Si tuviéramos que dormir en la misma cama, ¿cuál sería tu regla de oro?",
            "Pregunta: ¿Qué es lo más loco que has hecho por aburrimiento sexual?",
            "Reto: Dale un beso en la mejilla de 5 segundos a quien más te haya sorprendido hoy.",
            "Pregunta: ¿Qué verdad estás evitando aceptar sobre tu situación sentimental?",
            "Pregunta: ¿Qué es lo que más te hace reír en la intimidad?",
            "Reto: Muestra tu cara de 'máximo placer'.",
            "Pregunta: ¿Qué es lo que más te atrae de la inteligencia de alguien?",
            "Pregunta: ¿A quién de aquí le pedirías un consejo sexual?",
            "Reto: Intercambia una prenda o accesorio con la persona a tu derecha.",
            "Pregunta: ¿Qué es lo que más te hace sentir que perteneces a alguien?",
            "Pregunta: ¿Qué harías si alguien de aquí te propusiera un trío ahora mismo?",
            "Reto: Recibe un 'castigo' leve de la persona que elijas.",
            "Pregunta: ¿Qué es lo que más te ha gustado de este juego hasta ahora?",
            "Pregunta Final: Si pudieras repetir una noche de tu vida, ¿con quién sería?",
            "Reto Final: Brinda por la verdad que más te dolió decir hoy.",
            "El Pacto: Digan qué es lo que más les atrae de la persona a su derecha antes de cerrar."
        ];

        function nextCard() {
            const cardElement = document.getElementById('card');
            const contentElement = document.getElementById('content');
            cardElement.style.transform = "scale(0.8) rotateY(10deg)";
            setTimeout(() => {
                const randomIndex = Math.floor(Math.random() * cards.length);
                contentElement.innerText = cards[randomIndex];
                cardElement.style.transform = "scale(1) rotateY(0deg)";
            }, 200);
        }
    </script>
</body>
</html>
