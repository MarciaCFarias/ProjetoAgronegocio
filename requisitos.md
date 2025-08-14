# 📋 Requisitos do Processo – Faturamento Troca de Notas

## Regras de Negócio
- O robô deve processar **somente** chamados cujo assunto esteja configurado como Troca de Nota.
- Integrações devem ocorrer via **Webhook**, **XML**, **JSON** e **APIs** definidas (Sistema de chamdo, Contrele de Carregamento, SAP, OPUS, SIGAM, SITES DE AGENDAMENTO DE TRANSPORTE).
- Uso obrigatório da planilha **DE_PARA PARAMETRIZAÇÃO** para:
  - Classificação de produtos.
  - Identificação de veículos e transgenia.
  - Regras de agendamento e prazos.
- Dados do **Laudo** (XML) e do **Chamado** (JSON) devem ser validados e cruzados com a Ordem de Carregamento.
- Campos obrigatórios (ex.: `num_ov_embarcador`, chave NF-e, placa) devem existir e estar consistentes.
- Identificação de transportadora vs. **terceiros** deve seguir regras de CNPJ no DE_PARA.
- Reprocessamento deve seguir fila priorizada:
  1. Itens em fila de reprocessamento.
  2. Lançamentos pendentes.
- Alterações em arquivos, campos, credenciais ou tags devem ser comunicadas previamente; caso contrário, ficam fora do escopo.

---

## Validações Necessárias
- **Chamados**
  - Verificar campos `TotalAcoes`, `CodChamado`, `assunto_cod`.
  - Validar se o chamado atende aos critérios para processamento.
- **XML do Laudo**
  - Conferir tags: `<num_os>`, `<placa>`, `<peso_liq>`, `<peso_tara>`, `<peso_bruto>`, `<produto>`, `<tipo_produto>`, `<nota_fiscal_origem>`.
  - Validar blocos de classificação `<itens_class>` usando DE_PARA.
- **JSON do Chamado**
  - Conferir IDs e campos de identificação (ticket, placa, produto, pesos, chave NF-e, código da classificadora).
  - Validar classificações (% umidade, impurezas, avariados etc.) contra DE_PARA.
- **Ordem de Carregamento (OC)**
  - Validar placa, status (`A. Carregamento`), CNPJs, inscrição estadual (9 dígitos).
  - Conferir origem/destino, tarifas e dados do motorista.
- **SAP**
  - Validar safra, ref. cliente, CNPJ participante, datas de agendamento, parceiros.
- **Planilha DE_PARA**
  - Conferir todos os dados necessários de classificação, prazos e transgenia conforme ano/safra.
- **Regras de Agendamento**
  - Aplicar cálculo de data conforme texto (`#D+0`, `#D+1`, etc.) ou regras de KM x Dias na planilha.

---

## Saídas Esperadas
- **Integração concluída** com todos os sistemas previstos (Webhook, APIs, SAP, OPUS, SIGAM, SITES DE AGENDAMENTO DE TRANSPORTE).
- Dados processados e armazenados em **base de dados do cliente**, com:
  - Informações completas do chamado.
  - Dados validados e consistentes com regras de negócio.
- Lançamentos realizados nos sistemas correspondentes.
- Agendamentos efetuados conforme prazos e regras.
- Logs gerados para:
  - Sucesso.
  - Exceções de Sistema/técnica.
  - Exceções de negócio.
- Itens com erro direcionados automaticamente para **fila de reprocessamento**.

---

## Critérios de Sucesso
- **Taxa de Processamento Automático ≥ 95%** dos chamados elegíveis.
- **Zero falhas críticas** por inconsistência de dados que possam comprometer lançamentos fiscais.
- **Tempo médio de processamento** por chamado inferior ao registrado no processo manual (35 min).
- **Reprocessamento automático bem-sucedido** para > 90% dos casos em fila.
- **Integração estável** com todos os sistemas externos durante períodos de execução.
- **Base de dados** atualizada com 100% das informações necessárias para análises internas.
- **Conformidade** com regras fiscais e operacionais definidas no DE_PARA.

---

