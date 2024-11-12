#include <WiFi.h>
#include <ESP32Camera.h>
#include <HTTPClient.h>

// Configurações de rede Wi-Fi
const char* ssid = "SEU_SSID";
const char* password = "SUA_SENHA";

// Configurações do servidor
const char* serverURL = "http://seuservidor.com/upload";
const char* notificationURL = "http://seuservidor.com/notificar";

// Configurações de hardware
int intercomButtonPin = 2; // Botão do interfone
int doorUnlockPin = 3;     // Pin para destravar a porta

void setup() {
    Serial.begin(115200);
    WiFi.begin(ssid, password);

    // Configuração dos pinos
    pinMode(intercomButtonPin, INPUT);
    pinMode(doorUnlockPin, OUTPUT);
    digitalWrite(doorUnlockPin, LOW); // Porta bloqueada por padrão

    // Inicializar a câmera
    if (!ESP32Camera.begin()) {
        Serial.println("Erro ao inicializar a câmera");
        while (1); // Pausa se a câmera não inicializar
    }

    // Conectando ao Wi-Fi
    while (WiFi.status() != WL_CONNECTED) {
        delay(500);
        Serial.print(".");
    }
    Serial.println("Wi-Fi conectado");
}

void loop() {
    // Detecta se o botão do interfone foi pressionado
    if (digitalRead(intercomButtonPin) == HIGH) {
        // Captura e exibe imagem
        capturarImagem();
        
        // Processa autorização
        bool autorizado = verificarAutorizacao();
        
        if (autorizado) {
            destravarPorta();
        } else {
            enviarNotificacao();
        }
        
        // Armazena a imagem no servidor
        armazenarImagemServidor();
    }
}

// Função para capturar e exibir a imagem
void capturarImagem() {
    Serial.println("Capturando imagem...");
    camera_fb_t *fb = ESP32Camera.capture();
    if (fb) {
        // Função para enviar a imagem ao monitor
        enviarParaMonitor(fb);
    }
    ESP32Camera.release(fb);
}

// Enviar a imagem capturada ao monitor
void enviarParaMonitor(camera_fb_t *fb) {
    // Código para exibir a imagem em um monitor
    Serial.println("Imagem enviada ao monitor");
}

// Função para verificar se a pessoa é autorizada
bool verificarAutorizacao() {
    // Placeholder: Código de verificação
    // Em um sistema real, este pode ser uma API ou um processamento de reconhecimento facial.
    Serial.println("Verificando autorização...");
    return false; // Altere para simular acesso autorizado
}

// Função para destravar a porta
void destravarPorta() {
    Serial.println("Autorizado. Destravando porta...");
    digitalWrite(doorUnlockPin, HIGH);
    delay(5000); // Mantém a porta destravada por 5 segundos
    digitalWrite(doorUnlockPin, LOW); // Trava novamente a porta
}

// Função para enviar notificação de segurança
void enviarNotificacao() {
    Serial.println("Não autorizado. Enviando notificação de segurança...");
    HTTPClient http;
    http.begin(notificationURL);
    http.addHeader("Content-Type", "application/json");
    int httpResponseCode = http.POST("{ \"mensagem\": \"Acesso não autorizado detectado\" }");
    if (httpResponseCode > 0) {
        Serial.println("Notificação enviada");
    }
    http.end();
}

// Função para armazenar a imagem no servidor
void armazenarImagemServidor() {
    Serial.println("Armazenando imagem no servidor...");
    HTTPClient http;
    http.begin(serverURL);
    http.addHeader("Content-Type", "image/jpeg");
    // Enviar imagem (substituir pelo buffer da imagem)
    int httpResponseCode = http.POST((uint8_t*) fb->buf, fb->len); // Exemplo para enviar o buffer da imagem
    if (httpResponseCode > 0) {
        Serial.println("Imagem armazenada com sucesso");
    }
    http.end();
}






Configuração de Wi-Fi: Conexão com a rede para permitir envio de dados ao servidor e notificações.

Detecção de toque no interfone: O botão do interfone é conectado ao pino configurado e, ao ser pressionado, o sistema ativa o monitoramento.

Captura e exibição da imagem: Ao pressionar o botão, a câmera captura uma imagem que é enviada ao monitor.

Verificação de autorização: Após capturar a imagem, a função verificarAutorizacao() determina se a pessoa é autorizada. Isso pode ser expandido para incluir reconhecimento facial, biometria ou autenticação baseada em servidor.

Destravamento da porta: Se a pessoa for autorizada, o sistema destrava a porta por um período definido.

Notificação de segurança: Caso não autorizado, envia uma notificação ao servidor para alertar a equipe de segurança.

Armazenamento da imagem: A imagem capturada é enviada para um servidor onde será armazenada para fins de auditoria.

Expansão do Sistema
Para um sistema de produção, alguns detalhes adicionais poderiam ser incluídos:

Autenticação segura: Uso de tokens para API e HTTPS para comunicação segura.
Sistema de autenticação biométrica: Reconhecimento facial via modelos de IA, caso o hardware permita.
Configurações para ajuste de tempos: Variáveis para tempo de porta destravada e temporizadores de envio de notificações.
Gerenciamento de log: Armazenamento de eventos locais ou envio ao servidor de eventos para auditoria.
