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
