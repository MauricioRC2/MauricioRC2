<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mi Página Web con Chatbot</title>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@picocss/pico@1/css/pico.min.css">
    <style>
        #chatbox {
            max-width: 600px;
            margin: 0 auto;
            padding: 1em;
            border: 1px solid #ccc;
            border-radius: 5px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }
        .message {
            margin-bottom: 1em;
        }
        .user-message {
            text-align: right;
        }
    </style>
</head>
<body>
    <nav class="container-fluid">
        <ul>
            <li><strong>Mi Marca</strong></li>
        </ul>
        <ul>
            <li><a href="#">Inicio</a></li>
            <li><a href="#">Acerca de</a></li>
            <li><a href="#" role="button">Contacto</a></li>
        </ul>
    </nav>
    
    <main class="container">
        <div id="chatbox">
            <div class="message bot-message">Hola, ¿en qué puedo ayudarte hoy?</div>
        </div>
        <form id="chat-form">
            <input type="text" id="user-input" placeholder="Escribe tu mensaje..." required>
            <button type="submit">Enviar</button>
        </form>
    </main>

    <footer class="container">
        <small>
            <a href="#">Enlace 1</a> • <a href="#">Enlace 2</a>
        </small>
    </footer>

    <script>
        async function sendMessage(message) {
            const response = await fetch('https://api.openai.com/v1/engines/davinci-codex/completions', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                    'Authorization': 'Bearer YOUR_API_KEY'
                },
                body: JSON.stringify({
                    prompt: message,
                    max_tokens: 150
                })
            });
            const data = await response.json();
            return data.choices[0].text.trim();
        }

        document.getElementById('chat-form').addEventListener('submit', async (e) => {
            e.preventDefault();
            const userInput = document.getElementById('user-input').value;
            const chatbox = document.getElementById('chatbox');
            
            // Add user message to chatbox
            const userMessage = document.createElement('div');
            userMessage.className = 'message user-message';
            userMessage.textContent = userInput;
            chatbox.appendChild(userMessage);
            
            // Send message to API and get response
            const botResponse = await sendMessage(userInput);
            
            // Add bot response to chatbox
            const botMessage = document.createElement('div');
            botMessage.className = 'message bot-message';
            botMessage.textContent = botResponse;
            chatbox.appendChild(botMessage);
            
            // Clear user input
            document.getElementById('user-input').value = '';
        });
    </script>
</body>
</html>
