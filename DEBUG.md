# 🔧 Debugging Guide - WebRTC Calls

## 🚀 Como Usar a Aplicação

### 1. Criar uma Sala
- Acesse a página inicial
- Clique em "📹 Create New Room"
- Você será redirecionado para uma sala com ID único

### 2. Entrar em uma Sala Existente
- Na página inicial, digite o Room ID no campo "Room ID"
- Clique em "🚪 Join Room"
- Você será redirecionado para a sala

### 3. Fazer uma Chamada
- Na sala, clique em "📞 Start Call"
- Permita acesso à câmera e microfone quando solicitado
- Aguarde outros usuários entrarem na sala para conectar

## 🐛 Problemas Comuns e Soluções

### ❌ "Não consegui entrar na sala"

**Possíveis Causas:**
- Room ID incorreto
- Problemas de conectividade de rede
- Variáveis de ambiente não configuradas

**Soluções:**
1. Verifique se o Room ID está correto (sem espaços extras)
2. Abra o Console do Navegador (F12) e verifique erros
3. Certifique-se que as variáveis de ambiente estão configuradas

### ❌ "Não consegui fazer uma chamada"

**Possíveis Causas:**
- Permissões de câmera/microfone negadas
- Problemas de conexão Supabase
- Problemas de firewall/NAT

**Soluções:**
1. **Verificar Permissões:**
   - Clique no ícone de câmera na barra de endereço
   - Permitir acesso à câmera e microfone
   - Recarregue a página

2. **Verificar Console:**
   ```javascript
   // Abra F12 e verifique mensagens como:
   "Setting up signaling channel on mount"
   "Channel subscription status: SUBSCRIBED"
   "Sending offer: ..."
   ```

3. **Testar Conectividade:**
   - Teste em duas abas diferentes do navegador
   - Use modo incógnito para evitar cache
   - Teste em navegadores diferentes

### ❌ "Vejo meu vídeo mas não o do outro usuário"

**Possíveis Causas:**
- Problemas de ICE candidates
- Configuração de TURN server necessária
- Problemas de NAT/Firewall

**Soluções:**
1. **Verificar Logs ICE:**
   ```javascript
   // No console, procure por:
   "ICE connection state changed: checking"
   "ICE connection state changed: connected"
   "Received ICE candidate: ..."
   ```

2. **Configurar TURN Server** (para produção):
   ```env
   NEXT_PUBLIC_TURN_URL=turn:your-turn-server.com:3478
   NEXT_PUBLIC_TURN_USER=username
   NEXT_PUBLIC_TURN_PASS=password
   ```

## 🔍 Debug no Console

### Mensagens Esperadas (em ordem):
```
1. "Setting up signaling channel on mount"
2. "Channel subscription status: SUBSCRIBED"
3. "Requesting user media - audio: true video: true"
4. "Sending offer: ..." (quando clicar em Start Call)
5. "ICE connection state changed: checking"
6. "ICE connection state changed: connected"
```

### Comandos Úteis no Console:
```javascript
// Verificar estado da conexão
console.log('Peer connection state:', peerConnection.current?.connectionState);

// Verificar ICE connection state
console.log('ICE connection state:', peerConnection.current?.iceConnectionState);

// Verificar se o canal Supabase está ativo
console.log('Channel active:', channel.current?.state);
```

## 🌐 Variáveis de Ambiente Necessárias

### Para o Vercel:
```env
NEXT_PUBLIC_SUPABASE_URL=https://your-project.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-anon-key
```

### Opcionais (TURN Server):
```env
NEXT_PUBLIC_TURN_URL=turn:your-turn-server.com:3478
NEXT_PUBLIC_TURN_USER=username
NEXT_PUBLIC_TURN_PASS=password
```

## 🔧 Teste Local

### 1. Clonar e instalar:
```bash
git clone https://github.com/juliocamposmachado/WebRTC-Calls.git
cd WebRTC-Calls
npm install
```

### 2. Configurar .env.local:
```env
NEXT_PUBLIC_SUPABASE_URL=https://your-project.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-anon-key
```

### 3. Executar:
```bash
npm run dev
```

### 4. Testar:
- Abrir http://localhost:3000
- Criar uma sala
- Abrir em outra aba/navegador
- Entrar na mesma sala
- Fazer chamada

## 📱 Compatibilidade

### Navegadores Suportados:
- ✅ Chrome 80+
- ✅ Firefox 75+
- ✅ Safari 14+
- ✅ Edge 80+

### Funcionalidades Requeridas:
- WebRTC (RTCPeerConnection)
- getUserMedia (câmera/microfone)
- WebSocket (para Supabase Realtime)

## 🆘 Problemas Persistentes

Se ainda houver problemas:

1. **Verificar Supabase:**
   - Acesse o dashboard do Supabase
   - Verifique se Realtime está habilitado
   - Confirme as chaves de API

2. **Verificar Rede:**
   - Teste em rede diferente
   - Verificar se não há proxy/firewall bloqueando

3. **Logs Detalhados:**
   - Salve os logs do console
   - Verifique erros específicos
   - Compartilhe detalhes técnicos

## 📞 Fluxo de Conexão WebRTC

1. **Usuário A** entra na sala → canal Supabase criado
2. **Usuário B** entra na mesma sala → conecta ao canal
3. **Usuário A** clica "Start Call" → cria offer
4. **Usuário B** recebe offer → cria answer automaticamente
5. **ICE candidates** são trocados → conexão P2P estabelecida
6. **Streams de vídeo** são conectados → chamada ativa

---

✅ **Build testado e funcional**
🔧 **Todos os erros de TypeScript corrigidos**
🚀 **Pronto para deploy no Vercel**
