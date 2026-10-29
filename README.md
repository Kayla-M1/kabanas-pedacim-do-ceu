# Kabanas Pedacim do Céu: site de reservas (projeto conceito)

Site de uma página para uma hospedagem com duas cabanas em formato A no meio da mata. Fiz como **proposta para um negócio real**, que não chegou a ir ao ar. Nesta versão, o WhatsApp é o meu e os valores são ilustrativos.

**🔗 Ver online:** https://kayla-m1.github.io/kabanas-pedacim-do-ceu/

## O problema

O estabelecimento recebia reservas só pelo Instagram e pelo WhatsApp, e os preços ficavam numa imagem com regras diferentes para cada cabana (dias de chegada, horários de entrada e saída, pacotes de fim de semana, capacidade). Os clientes precisavam perguntar tudo antes de reservar.

## O que construí

- **Planejador de estadia.** O hóspede escolhe a data de chegada, quantos adultos e crianças vêm e qual cabana quer. A página mostra só os pacotes válidos para aquele dia da semana, com as datas de entrada e saída já calculadas, e respeita a capacidade de cada cabana. Quando não há opção para a data, sugere a próxima data com vaga.
- **Reserva com mensagem pronta no WhatsApp.** A mensagem já sai com a cabana, o pacote, o valor, as datas e o número de pessoas, sem backend e sem plataforma paga.
- **Formulário de contato** que confere os campos, envia o pedido para a automação com IA e mostra na hora a resposta sugerida, além de montar a mensagem para o WhatsApp.
- **Galeria em tela cheia** com teclado e gesto de deslizar no celular.
- Layout responsivo, acessibilidade (rótulos, foco visível, `prefers-reduced-motion`) e animações leves.

## 🤖 Automação com IA (n8n)

O formulário de contato está ligado a uma **automação no n8n**. A IA classifica cada pedido, define a prioridade, escreve uma resposta e registra tudo no Google Sheets, além de avisar a equipe por e-mail. A automação passou por quatro versões de engenharia de prompt para eliminar respostas inventadas pela IA.

👉 **[Veja o fluxo, os testes e a evolução v1 → v4](automacao-n8n/README.md)**

## Como a IA entrou no processo

Usei IA generativa (Claude, ChatGPT e Gemini) como parceira em todas as etapas:

1. Transformei a tabela de preços, que estava em imagem, em dados estruturados (`KABANAS` no código), conferindo os pontos em que a leitura ficou incerta.
2. Defini as regras de negócio (dias da semana, horários, capacidade) e pedi a lógica do planejador a partir delas.
3. Revisei o design, os textos e o comportamento no celular, ajustando até a página ficar pronta para apresentar ao cliente.

## Tecnologias

HTML, CSS e JavaScript puro, sem frameworks. O backend é a automação no n8n. A configuração fica no topo do `<script>`: o número do WhatsApp e a lista de cabanas, com fotos, pacotes, preços e regras.

## Estrutura

```
index.html   → página completa (HTML, CSS e JS)
fotos/       → imagens das cabanas
```

---

Feito por **Kayla Michelle**, estudante de Engenharia da Computação (Fametro, Manaus). As fotos pertencem ao estabelecimento e foram usadas apenas para fins de demonstração.
