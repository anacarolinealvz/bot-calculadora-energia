##  Bot de Controle de Consumo de Energia (Telegram + n8n)

Este projeto nasceu de um problema bem real: **nunca saber quanto a conta de luz vai vir no fim do mês**.  
A ideia foi criar um **assistente automatizado no Telegram** que ajudasse a registrar aparelhos, calcular o consumo diário e estimar o valor da conta ao longo do mês.

O bot foi desenvolvido usando **n8n**, com lógica em **JavaScript**, e utiliza o **Google Sheets** como banco de dados para manter tudo simples e acessível.


## 🎯 Objetivo
Ajudar a **prever o valor da conta de energia antes do fechamento**, permitindo cadastrar aparelhos de forma flexível:
- usando o consumo mensal da etiqueta do **INMETRO**
- ou informando manualmente **potência (Watts)** e **horas de uso diário**

Assim, o cálculo fica mais próximo da realidade de uso.


## 🛠️ Tecnologias utilizadas
- **n8n**: orquestração do fluxo e controle da lógica
- **JavaScript (ES6)**: cálculos, validações e tratamento de erros dentro dos nós *Code*
- **Telegram Bot API**: interface principal de interação
- **Google Sheets**: persistência dos dados (cadastro e consumo)


## ⚙️ Funcionalidades atuais
- Menu interativo no Telegram para cadastro e consulta
- Cadastro híbrido de aparelhos:
  - por etiqueta (kWh/mês)
  - ou por potência (Watts + horas/dia)
- Validação de entradas para evitar dados inválidos
- Cálculo proporcional do consumo acumulado do mês, considerando a tarifa informada

---

## 🚧 Roadmap (Próximos passos)
Este projeto ainda está em evolução. Algumas melhorias planejadas:

- Reset automático mensal do consumo
- Edição e exclusão de aparelhos cadastrados
- Geração de gráficos simples de consumo
- Suporte a bandeiras tarifárias
- Migração do Google Sheets para banco SQL
- Modo simulação para estimar o consumo de um aparelho antes da compra
- Refinamento da lógica para diferenciar aparelhos de uso contínuo e uso variável

---

## 📂 Como testar o fluxo
 **Documentação**
  - [Planilha do projeto](https://docs.google.com/spreadsheets/d/1hYlX-hOe7u7LG2F9Y9hHmp20jfzxrsctKg9aTtRBZzE/edit?usp=sharing)

O workflow principal está disponível neste repositório.

1. Instale o n8n (localmente ou via Docker)
2. Importe o arquivo `.json`
3. Configure as credenciais do Telegram e do Google Sheets
4. Crie uma planilha com as colunas:
   - `Nome`
   - `Etiqueta_kWh_Mes`
   - `Potencia_Watts`
   - `Horas_Dia`

---

## 🔧 Desafio técnico: execução local
Durante o desenvolvimento local, um dos maiores desafios foi integrar o **Telegram Trigger** com o n8n rodando em `localhost`.

A API do Telegram exige **HTTPS e IP público** para Webhooks, o que impede a comunicação direta com ambientes locais.

### Solução: ngrok
Para contornar isso sem subir o projeto em um servidor VPS durante a fase de desenvolvimento, utilizei o **ngrok**.

- O n8n não possui IP público nem HTTPS válido quando executado localmente
- O ngrok cria um túnel seguro, expondo a porta local (`5678`) para a internet
- Foi necessário configurar a variável de ambiente `WEBHOOK_URL` no n8n apontando para a URL gerada pelo ngrok

Com isso, o bot passou a receber mensagens do Telegram em tempo real mesmo rodando localmente.


