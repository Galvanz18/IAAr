# Campanha — Device Code Phishing (M365 / Entra ID) para Cofense

Kit de campanha de **conscientização intrusiva** (simulação de phishing) baseada
na técnica de *device code*. Projetado para rodar na **Cofense PhishMe** em um
engajamento **autorizado** contra a própria organização.

> ⚠️ **Modo conscientização, não comprometimento.** Esta campanha **não** inicia o
> fluxo OAuth real da Microsoft nem coleta tokens/credenciais. A isca leva ao
> ambiente da Cofense; quem "cai" vê a página educativa (`landing-teachable.html`).
> O `simulator/` é uma demonstração didática isolada, sem chamadas reais à Microsoft.

## Conteúdo

| Arquivo | Uso na Cofense |
|---|---|
| `email-template.html` | Template do e-mail isca (importar como *Email Template*). |
| `landing-teachable.html` | Página de "momento de ensino" (importar como *Landing/Education Page*). |
| `simulator/index.html` | Simulador interativo do fluxo — treino/awareness e demonstração em sala. Não é parte do envio; hospedar separadamente ou usar em apresentação. |
| `../../docs/device-code-phishing.md` | Base técnica, indicadores e mitigação. |

## Pré-requisitos / governança

- [ ] Autorização formal (escopo assinado) do engajamento.
- [ ] Janela de execução e lista de alvos aprovadas.
- [ ] SOC/Service Desk avisados (para não tratarem como incidente real).
- [ ] Página educativa revisada e aprovada pela área responsável.

## Tokens a substituir no template

O template usa placeholders. Troque pelos **data tokens** reais da sua instância
Cofense antes de publicar:

| Placeholder no HTML | Substituir por (Cofense) |
|---|---|
| `{{COFENSE_URL}}` | Token de URL rastreável da campanha (ex.: o link/URL variable da Cofense). |
| `{{RECIPIENT_NAME}}` | Token de nome do destinatário. |
| `{{RECIPIENT_EMAIL}}` | Token de e-mail do destinatário. |
| `{{FAKE_CODE}}` | Código "de exibição" (estático, ex.: `CFSN-DEMO`). **Não** gere código real de device flow. |
| `{{SENDER_DISPLAY}}` | Nome exibido do remetente configurado na campanha. |

> Os nomes exatos dos tokens variam conforme a versão/instância da Cofense
> (PhishMe). Consulte *Template Editor → Insert Variable* e mapeie os quatro acima.

## Roteiro operacional

1. **Importar** `email-template.html` e `landing-teachable.html` na Cofense.
2. **Mapear os tokens** da tabela acima.
3. **Configurar o cenário**: assunto, remetente, grupo-alvo e agendamento.
4. **Teste piloto** com o próprio time (seed list) antes do disparo amplo.
5. **Disparar** na janela aprovada.
6. **Medir** (ver abaixo) e **reportar**.

## Métricas sugeridas

- Taxa de abertura do e-mail.
- Taxa de clique na isca (chegou à landing).
- **Taxa de "digitou o código"** (interação simulada na landing) — principal KPI
  específico de device code: mede quem efetivamente seguiria o fluxo.
- Taxa de reporte (botão "Reportar phishing", se disponível) — sinal positivo.
- Tempo médio até o reporte.
- Reincidência vs. campanhas anteriores.

## Mensagem-chave de ensino

> **Nunca digite em `microsoft.com/devicelogin` um código que você recebeu de
> alguém.** Um login iniciado por você nunca chega pronto na sua caixa de entrada.
