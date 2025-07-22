### **🐞 Debugging em MySQL**

- MySQL não tem um debugger tradicional para procedures/triggers, mas:
    - Use **logs de erros**, tabelas de auditoria e de debug customizadas para rastrear execução
    - Utilize instruções SIGNAL para lançar erros customizados em triggers/procedures
    - Fragmentar queries complexas para localizar falhas

### **📊 EXPLAIN - Analisando Queries**

##### **Para que serve?**

- Analisar o plano de execução das queries SQL para identificar gargalos e otimizar performance

##### **Sintaxe Básica:**

sqlresponse-action-icon

```sql
EXPLAIN SELECT * FROM orders WHERE customer_id = 123;
```

##### **Exemplo Simplificado do Output:**

|id|select_type|table|type|possible_keys|key|rows|Extra|
|---|---|---|---|---|---|---|---|
|1|SIMPLE|orders|ref|idx_customer|idx_customer|10|Using index condition|

##### **Principais Colunas:**

- type: Tipo de junção / acesso (all, index, ref, eq_ref, const, system). Quanto mais próximo de **const/system**, melhor a performance.
- possible_keys: Índices que o otimizador considera usar.
- key: Índice efetivamente usado.
- rows: Estimativa de linhas percorridas.
- Extra: Informações adicionais como “Using where”, “Using temporary”, “Using filesort”, que indicam operações custosas.

##### **Visual Explain (Workbench)**

- Exibe o plano de execução numa forma gráfica, facilitando entendimento dos passos da query.

### **🔧 Técnicas de Debugging e Otimização:**

- Reescrever queries para usar índices
- Evitar SELECT *, preferir colunas específicas
- Analisar joins e subqueries para detectar scans desnecessários
- Utilizar partições e índices apropriados
- Verificar estatísticas e atualizar índices com ANALYZE TABLE
- Monitorar server status e slow query log
- Testar em ambiente de staging