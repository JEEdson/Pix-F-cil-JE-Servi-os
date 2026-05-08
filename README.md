# Pix-Facil-JE-Servicos
<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>Pix Fácil - Cole sua chave</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: system-ui, -apple-system, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
        }

        body {
            background: linear-gradient(145deg, #e0e7ff 0%, #c7d2fe 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 1rem;
        }

        .container {
            max-width: 550px;
            width: 100%;
            background: white;
            border-radius: 2rem;
            box-shadow: 0 20px 35px -10px rgba(0, 0, 0, 0.2);
            overflow: hidden;
            padding: 2rem 1.5rem 2rem 1.5rem;
            transition: all 0.2s ease;
        }

        h1 {
            font-size: 1.8rem;
            font-weight: 700;
            background: linear-gradient(135deg, #1e3c72, #2a5298);
            background-clip: text;
            -webkit-background-clip: text;
            color: transparent;
            margin-bottom: 0.5rem;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .sub {
            color: #4b5563;
            margin-bottom: 2rem;
            font-size: 0.95rem;
        }

        .pix-logo-mini {
            width: 32px;
            height: 32px;
            background: #32BCAD;
            border-radius: 50%;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            color: white;
            font-size: 1rem;
        }

        .input-group {
            margin-bottom: 1.5rem;
        }

        label {
            font-weight: 600;
            color: #1f2937;
            display: block;
            margin-bottom: 0.5rem;
        }

        textarea {
            width: 100%;
            padding: 1rem;
            font-size: 1rem;
            border: 2px solid #e2e8f0;
            border-radius: 1.5rem;
            background: #f9fafb;
            resize: vertical;
            font-family: monospace;
            transition: 0.2s;
        }

        textarea:focus {
            outline: none;
            border-color: #32BCAD;
            box-shadow: 0 0 0 3px rgba(50, 188, 173, 0.2);
            background: white;
        }

        /* Card da chave depois que colar */
        .pix-card {
            background: #f0fdfa;
            border: 1px solid #a7f3d0;
            border-radius: 1.5rem;
            padding: 1rem;
            margin: 1rem 0 1.5rem 0;
            display: flex;
            align-items: center;
            justify-content: space-between;
            flex-wrap: wrap;
            gap: 1rem;
            transition: all 0.3s ease;
        }

        .pix-info {
            display: flex;
            align-items: center;
            gap: 0.75rem;
            flex: 2;
            word-break: break-all;
        }

        .pix-icon {
            background: #32BCAD;
            width: 48px;
            height: 48px;
            border-radius: 30px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.7rem;
            box-shadow: 0 4px 8px rgba(0,0,0,0.05);
        }

        .pix-key {
            font-family: monospace;
            font-size: 0.95rem;
            font-weight: 500;
            color: #065f46;
            background: white;
            padding: 0.5rem 0.8rem;
            border-radius: 1rem;
            display: inline-block;
            max-width: 280px;
            overflow-x: auto;
            white-space: nowrap;
        }

        .copy-btn {
            background: #1e293b;
            border: none;
            padding: 0.7rem 1.4rem;
            border-radius: 2rem;
            font-weight: 600;
            color: white;
            display: inline-flex;
            align-items: center;
            gap: 0.5rem;
            cursor: pointer;
            transition: 0.2s;
            font-size: 0.9rem;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
        }

        .copy-btn:active {
            transform: scale(0.96);
        }

        .copy-btn.copied {
            background: #059669;
        }

        .hidden {
            display: none;
        }

        .toast-msg {
            position: fixed;
            bottom: 25px;
            left: 50%;
            transform: translateX(-50%);
            background: #1e293b;
            color: white;
            padding: 10px 20px;
            border-radius: 40px;
            font-size: 0.85rem;
            opacity: 0;
            transition: 0.2s;
            pointer-events: none;
            z-index: 999;
        }

        footer {
            margin-top: 2rem;
            text-align: center;
            font-size: 0.7rem;
            color: #6c757d;
        }
    </style>
</head>
<body>

<div class="container">
    <h1>
        <span class="pix-logo-mini">💠</span> Pix Rápido
    </h1>
    <div class="sub">Cole a chave Pix abaixo — o botão copiar aparecerá automaticamente</div>

    <div class="input-group">
        <label>📋 Sua chave Pix (CPF, e-mail, celular, aleatória)</label>
        <textarea id="pixInput" rows="2" placeholder="Ex: 11999999999&#10;ou&#10;seu@email.com&#10;ou&#10;123.456.789-00"></textarea>
    </div>

    <!-- Card que aparece APÓS colar algo -->
    <div id="pixCard" class="pix-card hidden">
        <div class="pix-info">
            <div class="pix-icon">💠</div>
            <div>
                <div style="font-weight:500; font-size:0.75rem; color:#047857;">CHAVE PIX</div>
                <span id="displayKey" class="pix-key"></span>
            </div>
        </div>
        <button id="copyButton" class="copy-btn">
            📋 Copiar
        </button>
    </div>

    <footer>
        ✅ Após colar a chave, pressione "Copiar" e cole no seu app do banco.
    </footer>
</div>

<div id="toast" class="toast-msg">✨ Chave copiada!</div>

<script>
    const pixInput = document.getElementById('pixInput');
    const pixCard = document.getElementById('pixCard');
    const displayKeySpan = document.getElementById('displayKey');
    const copyBtn = document.getElementById('copyButton');
    const toast = document.getElementById('toast');

    let currentKey = '';

    function showToast(message = '✨ Chave copiada!') {
        toast.textContent = message;
        toast.style.opacity = '1';
        setTimeout(() => {
            toast.style.opacity = '0';
        }, 1800);
    }

    function updatePixCard() {
        let rawKey = pixInput.value.trim();
        if (rawKey === '') {
            pixCard.classList.add('hidden');
            currentKey = '';
            return;
        }
        
        // Mostra o card com a chave
        currentKey = rawKey;
        displayKeySpan.textContent = rawKey;
        pixCard.classList.remove('hidden');
        
        // Reseta o estilo do botão
        copyBtn.classList.remove('copied');
        copyBtn.innerHTML = '📋 Copiar';
    }

    // Evento: sempre que o usuário digitar ou colar
    pixInput.addEventListener('input', function() {
        updatePixCard();
    });

    // Copiar a chave atual
    async function copyPixKey() {
        if (!currentKey) {
            showToast('❌ Nenhuma chave para copiar');
            return;
        }
        
        try {
            await navigator.clipboard.writeText(currentKey);
            copyBtn.classList.add('copied');
            copyBtn.innerHTML = '✅ Copiado!';
            showToast('🔁 Chave Pix copiada! Cole no banco.');
            
            // Voltar ao texto normal após 2 segundos
            setTimeout(() => {
                if (copyBtn.classList.contains('copied')) {
                    copyBtn.classList.remove('copied');
                    copyBtn.innerHTML = '📋 Copiar';
                }
            }, 2000);
        } catch (err) {
            showToast('❌ Erro ao copiar, tente manualmente');
        }
    }

    copyBtn.addEventListener('click', copyPixKey);

    // Se quiser que ao colar já destaque (apenas visual)
    // Também suportar colagem via botão direito
    window.addEventListener('load', () => {
        // exemplo inicial (opcional)
        updatePixCard();
    });
</script>
</body>
</html>
