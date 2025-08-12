# 📄 Mapeamento de Processo – Faturamento Troca de Notas

## Fluxo **TO BE**
O processo futuro (robotizado) foi estruturado em **SPRINTs**, com execução diária via **Webhook**, integrando dados de XML, JSON e APIs, conforme as etapas principais:

1. **Recebimento e Integração dos Chamados**
   - Captura de chamados via Webhook.
   - Validação de assunto e filtros específicos.
   - Acesso ao XML do laudo e JSON para coleta de dados.

2. **Captação e Validação de Dados**
   - Extração de informações essenciais (ticket, placa, pesos, produtos, NF-e, classificações).
   - Integração com API da **Ordem de Carregamento (OC)** e validações cruzadas.

3. **Consulta e Extração de Dados no SAP**
   - Consulta de safra, referência de cliente, datas de entrega e parceiros.
   - Identificação de CNPJ participante e parâmetros de agendamento.

4. **Leitura da Planilha DE_PARA**
   - Captura de regras e parâmetros padronizados.
   - Uso para mapeamento de classificação, veículos, transgenia e demais variáveis.

5. **Verificação do Tipo de Lançamento**
   - Identificação do cenário de operação (remessa, armazenagem, retorno, manifesto).
   - Execução de lançamentos e agendamentos nos sistemas SAP, OPUS, SIGAM e SITES DE AGENDAMENTO DE TRANSPORTES.

6. **Reprocessamento**
   - Execução de tratativas automáticas para exceções de negócio.
   - Priorização de filas de reprocessamento.

---

## Melhorias Implementadas no Processo
| Código | Descrição |
|--------|-----------|
| **M1** | Agilidade na captura e processamento dos chamados, integrando múltiplos sistemas. |
| **M2** | Redução de atividades manuais e repetitivas. |
| **M3** | Padronização das regras com planilha **DE_PARA** centralizada. |
| **M4** | Uso intensivo de Webhooks, XMLs e APIs para integrações rápidas. |
| **M5** | Armazenamento de dados em base estruturada para análise e melhoria contínua. |
| **M6** | Entregas em etapas (SPRINTs), permitindo ganhos imediatos. |
| **M7** | Implementação de módulo de reprocessamento com endpoints padronizados. |

---

## Tratativas em Caso de Exceções
O robô realiza **até 3 tentativas** em caso de falhas; se persistir, gera **LOG** e notifica o responsável.  

**Principais casos:**
1. Falha ao acessar diretórios, planilhas ou internet.  
2. Erros de login ou na execução de APIs.  
3. Dados obrigatórios ausentes (TAGs XML, campos JSON, planilhas).  
4. Divergências entre sistemas (ex.: placa, CNPJ participante).  
5. Situações não previstas → notificação via e-mail com print da tela.

---

## KPI – Indicadores Sugeridos
| Indicador | Descrição |
|-----------|-----------|
| **Tempo Médio de Processamento por Chamado** | Tempo em minutos para processar cada chamado. |
| **Taxa de Sucesso na Execução Automática** | % de chamados executados sem intervenção humana. |
| **Volume de Chamados Processados** | Quantidade diária ou mensal processada pelo robô. |
| **Tempo de Resposta ao Reprocessamento** | Tempo médio para reprocessar chamados com erro. |
| **Taxa de Erros por Tipo** | Distribuição de falhas por categoria (API, SAP, integração com plataforma de chamados). |
| **Redução de Horas Manuais** | Comparativo de tempo antes e depois da automação. |

---

## Observações
- O robô **não gera relatórios em planilhas**; todas as informações são enviadas a um banco de dados do cliente.  
- **Ambiente único (PRD)**: homologações devem ser acompanhadas e conciliadas com a operação real.  
- Alterações em arquivos, credenciais, campos ou tags devem ser comunicadas previamente para garantir funcionamento.  
- O uso de dados **DE_PARA** é crítico para o correto lançamento e agendamento.  
- O processo foi desenhado para ser resiliente, com fallback (estratégia de contingência) e reprocessamento automáticos.  

---

