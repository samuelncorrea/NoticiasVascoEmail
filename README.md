# Resumo diário de notícias do Vasco por e-mail (n8n)

Workflow do n8n que busca as notícias mais recentes sobre o **Vasco da Gama** no Google Notícias, monta um resumo em HTML e envia por e-mail todos os dias às 8h.

## Como funciona

```
Teste manual ─┐
              ├─> Ler feed RSS -> Limitar a 5 notícias -> Montar e-mail -> Enviar e-mail
Todo dia 8h  ─┘
```

| Nó | Função |
|---|---|
| Teste manual | Permite executar o fluxo na hora, para testar |
| Todo dia às 8h | Dispara o fluxo automaticamente toda manhã |
| Ler feed RSS | Busca no Google Notícias as notícias do Vasco das últimas 24h |
| Limitar a 5 notícias | Mantém só as 5 primeiras |
| Montar e-mail | Código JavaScript que gera o assunto e o corpo em HTML |
| Enviar e-mail | Envia o resumo por SMTP |

## Conceitos praticados

- Triggers (manual e por agendamento)
- Consumo de feed RSS
- Manipulação de dados com o nó Code
- Expressões do n8n (`{{ $json.campo }}`)
- Integração com e-mail (SMTP)

## Como usar

1. Abra o n8n e crie um novo workflow.
2. Menu `...` (canto superior direito) -> **Import from File** -> selecione `workflow.json`.
3. No nó **Enviar e-mail**:
   - crie uma credencial SMTP (no Gmail, use `smtp.gmail.com`, porta `465`, SSL ligado e uma *senha de app*);
   - troque `SEU_EMAIL@exemplo.com` pelo seu e-mail em "From" e "To".
4. Clique em **Execute workflow** para testar.
5. Para ativar o envio diário, ligue o botão **Active**.

### Trocar o time ou o assunto

A busca está na URL do nó **Ler feed RSS**:

```
https://news.google.com/rss/search?q=%22Vasco+da+Gama%22+when:1d&hl=pt-BR&gl=BR&ceid=BR:pt-419
```

- Troque `%22Vasco+da+Gama%22` por outro termo (ex.: `%22Flamengo%22`). O `%22` são aspas, para buscar o nome exato.
- `when:1d` limita às últimas 24 horas. Use `when:7d` para a última semana.

## Observações

- O arquivo não contém credenciais. Elas ficam guardadas no n8n.
- Ajuste o horário no nó **Todo dia às 8h**.
