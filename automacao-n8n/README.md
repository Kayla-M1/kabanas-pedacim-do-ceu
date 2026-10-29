# Automação de pedidos com IA (n8n)

Esta automação recebe os pedidos do formulário do [site das Kabanas](https://kayla-m1.github.io/kabanas-pedacim-do-ceu/#contato), usa IA para entender cada pedido e escrever uma resposta, salva tudo numa planilha e avisa a equipe por e-mail. **Funciona em produção:** quem preenche o formulário recebe a resposta da IA na hora, na própria página.

## O problema

Os pedidos chegavam soltos pelo WhatsApp. Ninguém sabia quais eram urgentes, as mesmas perguntas (preço, horário, quantas pessoas cabem) eram respondidas à mão o tempo todo, e não ficava registro de nada.

## O fluxo

```
Formulário do site ──► IA analisa o pedido ──► Organiza os dados ──► Google Sheets ──► Gmail ──► Resposta ao site
   (Webhook)          (GPT-4o-mini, JSON)        (Set)              (registro)       (aviso)    (aparece na página)
```

Para cada pedido, a IA devolve:

| Campo | Exemplo |
|---|---|
| `tipo` | reserva, dúvida, aniversário, grupo ou outro |
| `prioridade` | alta (chegada em até 7 dias ou data especial), média ou baixa |
| `resumo` | uma frase sobre o pedido, com o que falta confirmar |
| `resposta_sugerida` | a mensagem pronta para mandar no WhatsApp |

## Engenharia de prompt: v1 → v4

Testei a IA com pedidos reais e fui corrigindo cada falha sem quebrar o que já funcionava.

| Versão | O que mudou | Resultado no teste |
|---|---|---|
| **v1** | Instruções básicas | ❌ **Inventou uma regra.** Pergunta: "Pode levar cachorro?" Resposta: *"Sim, aceitamos cachorros!"* (essa informação não existe) |
| **v2** | Lista de **informações oficiais** e a regra "não invente, diga que vai confirmar" | ⚠️ Parou de inventar, mas ficou **cautelosa demais**: *"Vou confirmar sobre o cachorro e o custo"*, sendo que o preço estava na lista |
| **v3** | **Few-shot prompting:** exemplos de resposta certa e errada | ✅ *"O fim de semana na Kabana do Lago custa R$ 2.000, de sexta às 16h até domingo às 17h. Vou confirmar se pode levar cachorro."* ⚠️ Mas, com 3 adultos e 2 crianças, mandou "confirmar a capacidade", sendo que cabem 4 adultos |
| **v4** | Regra para **comparar o número de hóspedes com a capacidade** | ✅ *"A diária da Kabana dos Hibiscos é R$ 650 e cabe 3 adultos e 2 crianças. Sobre o café da manhã, vou confirmar."* ✅ Grupo grande: *"O seu grupo de 5 adultos e 1 criança é maior que a capacidade. Podemos conversar com a equipe para encontrar uma solução!"* |

O que aprendi:

- **Dar à IA uma fonte de verdade** (as informações oficiais) é o que impede alucinação.
- **Regras sozinhas deixam a IA insegura demais.** Exemplos concretos (few-shot) ensinam o equilíbrio.
- **Cada correção pede um teste de regressão**, ou seja, repetir os casos antigos para garantir que nada quebrou.
- **A IA sugere, a equipe decide:** ela nunca confirma disponibilidade nem pagamento.

## Como usar

1. Crie uma conta no [n8n](https://n8n.io) e importe o `kabanas-reservas-n8n.json` (*Workflows → Import from file*).
2. Conecte as credenciais do OpenAI, do Google Sheets e do Gmail.
3. Crie uma planilha com as colunas: `Data do pedido, Nome, Telefone, Kabana, Chegada, Mensagem, Tipo, Prioridade, Resumo, Resposta sugerida, Status`.
4. No nó **Avisa por e-mail**, troque `SEU_EMAIL@exemplo.com` pelo seu e-mail.
5. Publique o fluxo e coloque a URL do webhook na constante `PEDIDOS_URL` do `index.html`.

**Ferramentas:** n8n, OpenAI GPT-4o-mini, Google Sheets, Gmail, JavaScript (`fetch`) no site.

---

Feito por **Kayla Michelle**, estudante de Engenharia da Computação (Fametro, Manaus).
