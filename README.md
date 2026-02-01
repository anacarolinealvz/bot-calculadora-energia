# Bot de Controle de Consumo de Energia (Telegram + n8n)

![Status](https://img.shields.io/badge/Status-Em_Desenvolvimento-yellow) ![n8n](https://img.shields.io/badge/n8n-Workflow-orange) ![Telegram](https://img.shields.io/badge/Telegram-Bot-blue)

Um assistente pessoal automatizado para monitorar, registrar e estimar gastos com energia elétrica residencial, integrando lógica de programação (JavaScript) com banco de dados em planilha (Google Sheets).

## 🎯 Objetivo do Projeto
Resolver a dificuldade de prever o valor da conta de luz antes do fechamento do mês. O bot permite cadastrar aparelhos de forma híbrida (pela etiqueta do INMETRO ou potência manual) e calcula o custo acumulado em tempo real.

## 🛠️ Tecnologias Utilizadas
* **n8n (Workflow Automation):** Orquestração do fluxo e lógica de decisão.
* **JavaScript (ES6):** Manipulação de dados, tratamento de Strings e lógica matemática complexa dentro dos nós `Code`.
* **Telegram API:** Interface de interação com o usuário.
* **Google Sheets:** Banco de dados para persistência dos cadastros.

## ⚙️ Funcionalidades Atuais
*  **Menu Interativo:** Navegação via Switch para rotas de cadastro ou consulta.
*  **Cadastro Híbrido (Blindado):**
    * Via Etiqueta (kWh/mês) para eletrodomésticos padrão.
    * Via Manual (Watts + Horas/dia) para eletrônicos diversos.
*  **Tratamento de Erros:** O código previne falhas caso o usuário envie formatos incorretos ou dados vazios.
*  **Cálculo Proporcional:** Algoritmo que calcula o gasto acumulado do dia 1º até o dia atual (baseado na tarifa local).

## 🚧 Roadmap (Próximos Passos)
Este projeto está em evolução constante. As próximas melhorias planejadas são:
- [ ] **Reset Mensal:** Automação para arquivar os gastos no dia 30 e zerar o ciclo.
- [ ] **Edição de Itens:** Permitir que o usuário exclua ou edite um aparelho cadastrado errado.
- [ ] **Dashboard Visual:** Gerar um gráfico simples (imagem) do consumo por categoria.
- [ ] **Suporte a Bandeiras Tarifárias:** Adicionar multiplicador para bandeira amarela/vermelha.
- [ ] **Migração para SQL:** Substituir a planilha por um banco de dados relacional (PostgreSQL/Supabase) para garantir escalabilidade e integridade dos dados.
- [ ] **Modo Simulação (Test Drive):** Criar uma rota de cálculo temporária para estimar o custo de um aparelho antes da compra, sem persistir os dados no banco.
- [ ] **Refatoração da Regra de Negócio:** Ajustar o algoritmo para diferenciar "Aparelhos 24h" (baseados na etiqueta mensal) de "Uso Variável" (forçando o cálculo via Watts/Horas para TVs e Microondas).
- [ ] **Suporte a Bandeiras Tarifárias:** Implementar configuração dinâmica para multiplicar o custo conforme a bandeira vigente (Verde, Amarela, Vermelha).
- [ ] **Cálculo de Impostos Progressivos:** Refinar a fórmula matemática para considerar as faixas de ICMS que variam conforme o volume de consumo (ex: >150kWh).

## 📂 Como testar o fluxo
O arquivo principal da automação está disponível neste repositório como `workflow.json`.

1. Instale o [n8n](https://n8n.io/) localmente ou via Docker.
2. Importe o arquivo `.json`.
3. Configure suas credenciais (Telegram Bot Token e Google Sheets OAuth).
4. Crie uma planilha com as colunas: Nome, Etiqueta_kWh_Mes, Potencia_Watts, Horas_Dia.

- 📊 **Documentação**
  - [Planilha do projeto](https://docs.google.com/spreadsheets/d/1hYlX-hOe7u7LG2F9Y9hHmp20jfzxrsctKg9aTtRBZzE/edit?usp=sharing)

## 🔧 Desafios de Implementação Local (Simulação)

Como o projeto foi desenvolvido em ambiente local (localhost), um dos principais desafios técnicos foi configurar o **Telegram Trigger**.

A API do Telegram utiliza **Webhooks** para enviar as mensagens do usuário para o bot. Porém, por questões de segurança e arquitetura de rede, o Telegram não consegue enviar dados diretamente para uma máquina local (localhost:5678).

### Solução: Tunneling com ngrok
Para contornar essa limitação sem precisar subir o projeto para um servidor VPS pago durante a fase de desenvolvimento, utilizei o **ngrok**.

1. **O Problema:** O n8n rodando localmente não possui um IP público nem HTTPS válido, requisitos obrigatórios para o Webhook do Telegram.
2. **A Solução:** O ngrok criou um túnel seguro, expondo a porta local do n8n (5678) para a internet através de uma URL pública temporária (ex: https://xyz.ngrok-free.app).
3. **Configuração:** Foi necessário configurar a variável de ambiente `WEBHOOK_URL` no n8n apontando para o endereço gerado pelo ngrok, permitindo que o bot recebesse as mensagens em tempo real.

---
*Desenvolvido por Ana Assunção*
