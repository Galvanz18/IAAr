# Device Code Phishing (OAuth 2.0 Device Authorization Grant) — Microsoft 365 / Entra ID

> Documento técnico de apoio à campanha de conscientização. Uso exclusivo em
> engajamentos autorizados (purple team / programa interno de conscientização).

## 1. O que é

O **Device Authorization Grant** (RFC 8628) é um fluxo OAuth 2.0 legítimo criado
para dispositivos sem teclado/navegador cômodo (Smart TV, console, CLI, IoT).
O dispositivo exibe um **código curto** e pede que o usuário abra
`https://microsoft.com/devicelogin` em outro aparelho, faça login e digite o código.

No **abuso** (device code phishing), o atacante **inicia o fluxo** no lugar do
dispositivo legítimo e envia o código para a vítima por e-mail/chat, pedindo que
ela o insira na página **real** da Microsoft. Ao fazer isso, a vítima autoriza o
"dispositivo" do atacante — entregando tokens de acesso e de atualização.

## 2. Por que é perigoso

- **Domínio legítimo**: a vítima autentica em `login.microsoftonline.com` /
  `microsoft.com/devicelogin`. Não há domínio falso, certificado suspeito nem
  typosquatting — defesas tradicionais de anti-phishing não disparam.
- **MFA é satisfeito pela vítima**: o segundo fator é aprovado pela própria
  vítima durante o login real. O atacante herda a sessão já autenticada.
- **Tokens duradouros**: o refresh token pode valer semanas, permitindo acesso
  persistente mesmo após troca de senha (até revogação explícita da sessão).
- **Baixa fricção**: a vítima só precisa "digitar um código" — parece inócuo.

## 3. Fluxo técnico (visão do atacante)

```
1. POST https://login.microsoftonline.com/common/oauth2/v2.0/devicecode
      client_id=<client público, ex. app first-party>&scope=<escopos>
   -> resposta:
      {
        "user_code": "ABCD-EFGH",
        "device_code": "<opaco>",
        "verification_uri": "https://microsoft.com/devicelogin",
        "expires_in": 900,
        "interval": 5
      }

2. Atacante envia `user_code` à vítima (isca): "Digite ABCD-EFGH em
   microsoft.com/devicelogin para continuar."

3. Vítima abre a página REAL, autentica (inclui MFA) e digita o código ->
   autoriza o dispositivo do atacante.

4. Atacante faz polling do token endpoint:
   POST .../oauth2/v2.0/token
      grant_type=urn:ietf:params:oauth:grant-type:device_code&
      device_code=<opaco>&client_id=<...>
   -> recebe access_token + refresh_token.
```

> **Nesta campanha de conscientização nós NÃO executamos os passos 1 e 4.**
> A simulação para no ponto de ensino: o objetivo é treinar a pessoa a
> reconhecer e NÃO inserir códigos recebidos de terceiros. Nenhum token real é
> solicitado ou coletado. Veja `campaigns/device-code-m365/`.

## 4. Sinais que o usuário deve reconhecer (mensagem da campanha)

Um pedido de código é **legítimo apenas quando foi VOCÊ quem iniciou o login**
em um dispositivo (TV, console, ferramenta de linha de comando). É **phishing**
quando:

- O código chega por e-mail, chat, SMS ou ligação sem você ter iniciado nada.
- Há urgência ("seu acesso expira", "aprove em 10 minutos").
- A mensagem pede para "colar/digitar este código para liberar/continuar
  acesso a um documento, Teams, VPN, benefício, etc.".
- A pessoa que envia se passa por TI/suporte/RH e oferece "ajuda" com o código.

**Regra de ouro:** nunca digite em `microsoft.com/devicelogin` um código que
você recebeu de alguém. Um login iniciado por você nunca chega pronto na sua
caixa de entrada.

## 5. Detecção e mitigação (para o time de defesa)

- **Entra ID Sign-in logs**: filtre `Authentication Protocol = Device Code`.
  Logins device code a partir de IP/geolocalização inesperada são suspeitos.
- **Conditional Access**: crie política bloqueando o grant device code onde não
  for necessário (`Authentication flows` → Device code flow → Block), liberando
  apenas grupos/apps que realmente usam dispositivos sem navegador.
- **Revogação**: em caso de comprometimento, revogue sessões (`Revoke sign-in
  sessions`) — trocar a senha sozinho não invalida refresh tokens.
- **Alertas**: monitore picos de autenticações device code e consentimentos de
  apps incomuns.

## 6. Referências

- RFC 8628 — OAuth 2.0 Device Authorization Grant
- Microsoft Entra — Conditional Access: Authentication flows
- Microsoft Entra — Sign-in logs / authentication protocol filtering
