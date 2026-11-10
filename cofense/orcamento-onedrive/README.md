# Cenário Cofense PhishMe — OneDrive "Orçamento Confidencial do CEO"

Template de **simulação de phishing** para formação de consciencialização, adaptado para a plataforma **Cofense PhishMe**.

## Ficheiros

| Ficheiro | Uso na Cofense |
|---|---|
| `email.html` | Template de **Email** do cenário |
| `landing-page.html` | **Landing page** (lure de *device code*) |

## Tokens de dados Cofense

A plataforma substitui estes tokens no momento do envio:

| Token | Significado |
|---|---|
| `!First_Name!` | Primeiro nome do destinatário |
| `!Last_Name!` | Apelido |
| `!Email!` | Email do destinatário |

O botão **"Abrir documento"** usa `{{.URL}}`. Em alternativa, no editor
Cofense selecione o botão e aplique o **Phishing Hook** — a plataforma
reescreve o link e trata do *tracking* de cliques automaticamente.

## Vetores de engenharia social (para o relatório)

- **Autoridade** — alegada partilha pelo Diretor-Geral/CEO.
- **Urgência** — "com urgência, ainda hoje".
- **Confidencialidade** — "ficheiro confidencial", "não reencaminhe".
- **Marca de confiança** — imagem Microsoft/OneDrive.
- **Técnica avançada** — *device code phishing* (código `AN62-83B9`).

## Sinais de alerta a ensinar no *teachable moment*

- Pedido de "código de verificação" copiado para um portal externo.
- Pressão de tempo e sigilo pouco habituais.
- Remetente/caixa "não monitorizada" para uma partilha supostamente pessoal.
- Verificar sempre o domínio real da página de início de sessão Microsoft.

> **Importante:** configure o fluxo para terminar na **página educativa**
> da Cofense após a interação (nenhuma credencial ou código é recolhido).
