# README – Bot Telegram SgarbiBot (PHP)

Questo progetto crea un bot Telegram che risponde con citazioni del famoso diverbio tra Sgarbi, Mosca e Squitieri quando rileva alcune parole chiave nei messaggi della chat.

***

### Funzionalità
- Monitora i messaggi in gruppo o chat privata
- Riconosce parole chiave predefinite
- Risponde con una citazione associata scelta casualmente
- Facile personalizzazione delle citazioni e delle parole chiave

***

### Requisiti
- PHP 7.4 o superiore
- Hosting con supporto HTTPS per webhook
- Un bot Telegram creato tramite @BotFather (servirà il token)
- File `quotes.json` con citazioni e parole chiave

***

### Installazione
1. **Crea il bot su Telegram via BotFather. Prendi nota del token.**  
2. **Scarica/Clona il progetto.**
3. **Installa [irazasyed/telegram-bot-sdk](https://github.com/irazasyed/telegram-bot-sdk) usando Composer:**  
   ```bash
   composer require irazasyed/telegram-bot-sdk
   ```
4. **Metti il token nel file .env oppure direttamente nello script.**

***

### File quotes.json (esempio)
```json
{
  "sgarbi": [
    "Lei è un povero provocatore!",
    "La smetta di dire sciocchezze."
  ],
  "mosca": [
    "Non faccia il furbo con me!",
    "Le ricordo che qui non siamo al bar."
  ],
  "squitieri": [
    "Io non mi faccio zittire da nessuno!",
    "Senta, cerchi di avere rispetto."
  ]
}
```

***

### Esempio di codice (webhook.php)

```php
<?php
require 'vendor/autoload.php';

use Telegram\Bot\Api;

// Token Telegram
$token = 'INSERISCI_IL_TUO_TOKEN_BOT';
$telegram = new Api($token);

// Ricevi l'update dal webhook
$content = file_get_contents('php://input');
$update = json_decode($content, true);

if (isset($update['message'])) {
    $chatId = $update['message']['chat']['id'];
    $text = strtolower($update['message']['text']);
    $quotes = json_decode(file_get_contents('quotes.json'), true);

    foreach ($quotes as $keyword => $citations) {
        if (strpos($text, $keyword) !== false) {
            // Prendi una citazione a caso
            $quote = $citations[array_rand($citations)];
            $telegram->sendMessage([
                'chat_id' => $chatId,
                'text' => $quote
            ]);
            break;
        }
    }
}
?>
```

***

### Configurazione webhook

Imposta l’URL del webhook verso il tuo hosting/php:
```bash
curl -X POST "https://api.telegram.org/botTOKEN/setWebhook?url=https://TUOSITO.IT/webhook.php"
```
Sostituisci `TOKEN` con il tuo e l’URL col path effettivo del tuo script.

***

### Personalizzazione

- Aggiungi/cambia parole chiave e citazioni nel file `quotes.json`
- Puoi estendere la logica al file PHP per gestire altre funzioni/command

***

### Suggerimenti
- Puoi ospitare lo script su qualsiasi host PHP con HTTPS
- Testa sempre in chat privata prima di metterlo in gruppi reali

***

Vuoi anche la versione pronta per comando dalla shell che sfrutta il polling anziché webhook?

Fonti
[1] Telegram Bot in PHP https://www.html.it/pag/399872/telegrambot-in-php/
[2] API Telegram per example - LorenzoNeri.com https://lorenzoneri.com/api-telegram-per-example/
[3] Come realizzare in PHP un bot per il controllo di accesso ad ... https://www.barattalo.it/coding/come-realizzare-in-php-un-bot-per-il-controllo-di-accesso-ad-un-gruppo-telegram/
