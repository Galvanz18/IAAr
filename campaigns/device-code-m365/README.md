# Campanha — Device Code Phishing (M365 / Entra ID) para Cofense

Kit de **conscientização intrusiva** (simulação de phishing) baseada na técnica de
*device code*, para rodar no **Microsoft Attack Simulator** (Attack Simulation
Training) ou na **Cofense PhishMe**, num engajamento **autorizado** contra a
própria organização.

> ⚠️ **Sem JavaScript.** O Microsoft Attack Simulator **remove `<script>`/JS** das
> páginas (documentado para o editor de payloads; o `<style>` é mantido). Por isso
> as páginas são **só HTML/CSS**. Consequências:
> - **Não há auto-copiar** para o clipboard (impossível sem JS): o código é grande
>   e **selecionável** para cópia manual.
> - **Não há popup**: o "Continuar" é um link com `target="_blank"` (abre novo
>   separador), o equivalente sem-JS.
>
> Tags não suportadas no editor de payloads do Attack Simulator: `script`, `iframe`,
> `meta`, `title`, `link`, `object`, `embed`, `frame(set)`, `noscript`, entre outras.

> ⚠️ **Modo conscientização, não comprometimento.** Nada aqui inicia o fluxo OAuth
> real da Microsoft nem coleta tokens/credenciais. Os códigos são estáticos e
> fictícios; a submissão é inerte. O **treino é apresentado pela própria Cofense**
> após a interação.

## Estrutura (3 pontos)

| # | Ficheiro | Papel | Importar como |
|---|---|---|---|
| 1 | `01-email-ceo-fileshare.html` | E-mail isca: impersonação de **CEO/RH** a partilhar um ficheiro confidencial. | Payload / *Email Template* |
| 2 | `02-pagina-codigo.html` | Página que **exibe o código** de verificação ("ver documento partilhado"). | Landing Page |
| 3 | `03-pagina-submissao.html` | Página de **submissão do código** (simula `microsoft.com/devicelogin`). | Landing Page |

Fluxo: **E-mail (1)** → clique leva à **Página do código (2)** → "Continuar para a
Microsoft" (novo separador) leva à **Página de submissão (3)** → a plataforma
regista a interação e mostra o treino.

Apoio: `simulator/index.html` (demo didática do ataque) e
`../../docs/device-code-phishing.md` (base técnica, deteção e mitigação).

## Placeholders a substituir

| Placeholder | Onde | Substituir por |
|---|---|---|
| `{{RECIPIENT_NAME}}` | e-mail | Nome do destinatário (token dinâmico da plataforma). |
| `{{CEO_NAME}}` | e-mail | Nome do executivo/RH a impersonar (pretexto). |
| `{{COFENSE_URL}}` | e-mail | Link de phishing para a Página do código (2). |
| `PHISHING_URL_AQUI` | página 2 | Link de phishing que leva à Página de submissão (3). |
| `SUBMIT_URL_AQUI` | página 3 | Destino de submissão/registo da interação. |

**Como inserir o link, por plataforma:**

- **Microsoft Attack Simulator**: não escreva o link à mão — use o **controlo de
  link/URL de phishing do editor** (payload e landing). O nome do destinatário usa
  os **dynamic tags** da plataforma (ex.: `${userDisplayName}`). O registo de quem
  clicou/submeteu é **nativo** da ferramenta.
- **Cofense PhishMe**: substitua pelos **data tokens** da sua instância
  (ex.: URL rastreável e `${submitLink}`). Consulte *Template Editor → Insert Variable*.

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
