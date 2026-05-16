# Hardware Humano: O Sistema Operacional do Atleta Tech
### *Engenharia de Dados, Machine Learning e Business Intelligence aplicados à Saúde Fisiológica e Mental*

---

## Visão Geral do Projeto
O projeto **Hardware Humano** une a ciência da **Educação Física** com a rotina de alta performance cognitiva da **Área Tech**. Profissionais de tecnologia monitoram servidores 24/7, mas costumam negligenciar o próprio corpo. Utilizando o conceito de *Load Management* (Gestão de Carga), este projeto centraliza, limpa e analisa dados biométricos e psicológicos para mitigar riscos de *burnout* e estresse crônico.

---

## Arquitetura de Dados e Pipeline (End-to-End)

O projeto segue a evolução de uma arquitetura de dados moderna estruturada em três etapas lógicas de execução:

1. **Ingestão e Limpeza:** `load_data.ipynb`
   * Extração de dados brutos de alta frequência do ecossistema Fitbit (JSON) e questionários subjetivos PMSys (CSV).
   * Tratamento de dicionários aninhados, padronização de datas (padrão ISO 8601) e carga inicial em lote no SQLite3.
   * Migração por fluxo de blocos (*Streaming em Chunks* de 100k linhas) para o **PostgreSQL** para suportar a volumetria de milhões de linhas de séries temporais.

2. **Otimização e Análise:** `analises_postgres.ipynb`
   * Criação de **Materialized Views** indexadas no PostgreSQL utilizando **Índices Estruturados (B-Tree)** e **LATERAL JOINs**.
   * Otimização analítica que reduziu o tempo de cruzamento de dados de **9 horas para menos de 1 minuto**.
   * Cálculo da Frequência Cardíaca de Repouso (RHR) durante as janelas de sono cruzada com os níveis diários de estresse.

3. **Predição com Inteligência Artificial:** `modelagem_burnout.ipynb`
   * Treinamento de um classificador de Árvore de Decisão (*Decision Tree Classifier*) utilizando mais de **540.000 registros**.
   * O modelo utiliza métricas de recuperação biológica do sono atual para prever o risco de picos de estresse no dia seguinte, atingindo **83% de acurácia geral** e **93% de precisão** nos alertas críticos.

---

## Principais Insights Extraídos

* **Resiliência Cardíaca:** A correlação estatística entre o estresse mental diário e o aumento imediato de batimentos no repouso foi de **-0.04** (nula), provando que o coração tolera o estresse psicológico isolado durante o sono.
* **O Verdadeiro Vilão:** A correlação mais forte identificada foi entre o RHR e a eficiência do sono (**-0.29**). Se a qualidade do sono cai, o esforço cardíaco aumenta no repouso. O sono é o principal regulador do "atleta tech".
* **Volume de Alertas:** O algoritmo de monitoramento acionou o alarme vermelho (*Alerta: Focar em Saúde Mental*) em mais de **7.100 dias** da base histórica (11% do tempo total), demonstrando a necessidade urgente de gerenciamento preventivo nas empresas de tecnologia.

---

## Camada de Business Intelligence (Power BI)

O encerramento do pipeline de dados ocorre na camada de consumo visual. O arquivo `dashboards/painel_atleta_tech.pbix` conecta-se diretamente ao PostgreSQL por meio de DirectQuery/Import da Materialized View `mv_saude_tech_integrada`, garantindo que os dados analíticos sejam consumidos em tempo real sem sobrecarregar o servidor.

### 📸 Demonstração do Painel
![Demonstração do Dashboard](./pipeline%20ETL/dashboards/print_dashboard.png)

### Recursos Disponíveis no Dashboard:
* [cite_start]**Indicadores Executivos (KPIs):** Monitoramento instantâneo do total de Alertas Críticos de Saúde Mental ativados, média global da Qualidade do Sono e Frequência Cardíaca de Repouso (RHR) do time.
* [cite_start]**Gráfico de Dispersão/Linhas Temporal:** Cruzamento analítico dinâmico demonstrando o impacto fisiológico imediato (elevação do RHR em BPM) nos dias subsequentes a noites de sono ineficientes[cite: 1229].
* [cite_start]**Filtros por Segmentadores Interativos (Tiles):** Capacidade de isolar a análise de forma granular por Usuário (`userId` de 1 a 16) e por Níveis de Estresse Purificados (removendo registros nulos e em branco do painel por regras de Data Quality)[cite: 1230].

---

## Como Executar o Projeto Localmente

1. **Clonar o Repositório e Configurar Ambiente:**
   ```bash
   git clone [https://github.com/seu-usuario/hardware-humano.git](https://github.com/seu-usuario/hardware-humano.git)
   cd hardware-humano
   python -m venv .venv
   source .venv/bin/activate  # No Windows use: .venv\Scripts\activate
   pip install -r requirements.txt

2. **Configurar as Variáveis de Ambiente:**
    Copie o arquivo .env.example para .env e preencha as credenciais com os dados locais do seu PostgreSQL:
    ```bash
    cp .env.example .env

3. **Execução:**
    Execute os notebooks na ordem lógica do pipeline: 
        load_data.ipynb -> analises_postgres.ipynb -> modelagem_burnout.ipynb