# Campanha — Device Code Phishing (M365 / Entra ID) para Cofense

Kit de **conscientização intrusiva** (simulação de phishing) baseada na técnica de
*device code*, para rodar na **Cofense PhishMe** num engajamento **autorizado**
contra a própria organização.

> ⚠️ **Modo conscientização, não comprometimento.** Nada aqui inicia o fluxo OAuth
> real da Microsoft nem coleta tokens/credenciais. Os códigos são estáticos e
> fictícios; a submissão é inerte. O **treino é apresentado pela própria Cofense**
> após a interação.

## Estrutura (3 pontos)

| # | Ficheiro | Papel | Importar na Cofense como |
|---|---|---|---|
| 1 | `01-email-ceo-fileshare.html` | E-mail isca: impersonação de **CEO/RH** a partilhar um ficheiro confidencial. | *Email Template* |
| 2 | `02-pagina-codigo.html` | Página que **exibe o código** de verificação ("ver documento partilhado"). | *Landing Page* |
| 3 | `03-pagina-submissao.html` | Página de **submissão do código** (simula `microsoft.com/devicelogin`). | *Landing Page* |

Fluxo: **E-mail (1)** → clique leva à **Página do código (2)** → "Continuar para a
Microsoft" leva à **Página de submissão (3)** → Cofense regista a interação e mostra o treino.

Apoio: `simulator/index.html` (demo didática do ataque) e
`../../docs/device-code-phishing.md` (base técnica, deteção e mitigação).

## Tokens a substituir

Troque os placeholders pelos **data tokens** reais da sua instância Cofense:

| Placeholder | Onde | Substituir por |
|---|---|---|
| `{{RECIPIENT_NAME}}` | e-mail | Nome do destinatário. |
| `{{CEO_NAME}}` | e-mail | Nome do executivo/RH a impersonar (pretexto). |
| `{{COFENSE_URL}}` | e-mail | URL rastreável que aponta para a Página do código (2). |
| `{{COFENSE_SUBMIT_URL}}` | página 3 | Endpoint de tracking da submissão (ou use o tracking nativo e mantenha o envio inerte). |

Entre a página 2 e a 3, o botão "Continuar" usa um link relativo
(`03-pagina-submissao.html`). Ao hospedar na Cofense, ajuste para a URL/token da
página seguinte da campanha.

> Os nomes exatos dos tokens variam conforme a versão da Cofense (PhishMe).
> Consulte *Template Editor → Insert Variable*.

## Governança (antes de disparar)

- [ ] Autorização formal (escopo assinado) do engajamento.
- [ ] Janela de execução e lista de alvos aprovadas.
- [ ] SOC/Service Desk avisados (para não tratarem como incidente real).
- [ ] Teste piloto com seed list antes do disparo amplo.

## Métricas sugeridas

- Taxa de abertura e de clique (chegou à página 2).
- **Taxa de "submeteu o código"** na página 3 — KPI principal do device code.
- Taxa e tempo de reporte (botão de phishing) — sinal positivo.
- Reincidência vs. campanhas anteriores.

## Mensagem-chave de ensino (fornecida pela plataforma)

> **Nunca digite em `microsoft.com/devicelogin` um código que recebeu de alguém.**
> Um login iniciado por si nunca chega pronto à sua caixa de entrada.
