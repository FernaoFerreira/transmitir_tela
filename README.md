# Instant Local Stream 🚀

Aplicação simples e eficiente para transmissão de tela P2P (Peer-to-Peer) utilizando **WebRTC**, **Socket.IO** e **Cloudflare Tunnels**.

---

## 🛠️ Como Funciona

- **Host (`/`)**: O transmissor acessa a página do host, escolhe a qualidade (resolução e FPS) e inicia a transmissão da tela/áudio.
- **Viewers (`/watch/:token`)**: Os espectadores conectam-se diretamente via WebRTC com o host usando o link seguro gerado com token.
- **Túnel Automático**: O servidor tenta criar um túnel público via `cloudflared` e encurtar o link via TinyURL.

---

## 🚀 Como Executar

### 1. Instalar Dependências
```bash
npm install
```

### 2. Iniciar o Servidor
```bash
npm start
```
O servidor estará rodando em `http://localhost:8080`.

---

## 🌐 Configurações e Recursos Avançados

### 1. Suporte a Servidor TURN (Fallback para NAT Restrito)
Por padrão, a aplicação utiliza servidores STUN públicos da Google. Caso o host ou espectadores estejam atrás de NATs simétricos ou firewalls corporativos restritos, você pode definir variáveis de ambiente para incluir um servidor TURN:

```bash
TURN_URL="turn:seu-servidor-turn.com:3478" \
TURN_USERNAME="usuario" \
TURN_CREDENTIAL="senha" \
npm start
```

---

### 2. Modo Fallback de Rede Local (LAN)
Se o executável `cloudflared` não estiver instalado ou falhar em iniciar o túnel público, a aplicação entra automaticamente em modo de fallback e exibe o IP da rede local para acesso via LAN:

```text
========================================================
CLOUDFLARED INDISPONÍVEL / FALHOU
USANDO MODO DE REDE LOCAL (LAN FALLBACK):
Link para assistir na mesma rede: http://192.168.x.x:8080/watch/<token>
========================================================
```

---

### 3. Rotação de Link / Token (Segurança de Sala)
Para impedir que espectadores não autorizados entrem caso o link vaze:
1. No painel do Host, clique no botão **"Gerar Novo Link"**.
2. O servidor irá invalidar o token antigo imediatamente e desconectar todos os espectadores que estavam usando o link antigo.
3. Um novo link será gerado e exibido no painel do host e no console do servidor.
