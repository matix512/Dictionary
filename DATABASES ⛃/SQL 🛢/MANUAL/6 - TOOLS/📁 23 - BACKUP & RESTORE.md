### **📦 Backup em MySQL**

Backup é essencial para proteção de dados e recuperação ante falhas.

### **Tipos Principais de Backup:**

- **Logical Backup:** Exporta o esquema e dados em formato SQL (ex.: mysqldump)
    - Fácil leitura e edição
    - Pode ser lento para bases muito grandes
- **Physical Backup:** Cópia dos ficheiros físicos do servidor (ex.: InnoDB files, binlogs)
    - Rápido para grandes volumes
    - Requer consistência e parada controlada (ou ferramentas de snapshot)

---

### **Principais Ferramentas e Métodos:**

#### 1. **mysqldump**

Backup lógico padrão para tabelas e dados

```bash
mysqldump -u user -p database_name > backup.sql
```

- Backup completo ou parcial
- Pode exportar só estrutura (--no-data) ou dados
- Compatível com restauração via import SQL

#### Exemplo para backup completo com compressão:

```bash
mysqldump -u user -p database_name | gzip > backup_$(date +%F).sql.gz
```

---

#### 2. **mysqlpump** (MySQL 5.7+)

Mais rápido e com opções avançadas que mysqldump, suporta paralelismo

```bash
mysqlpump -u user -p database_name > backup.sql
```

---

#### 3. **Percona XtraBackup**

Backup físico sem necessidade de lock (InnoDB hot backup)

- Ideal para bases grandes e ambiente de produção
- Permite fazer backups consistentes sem downtime

---

### **📥 Restore (Restauração):**

#### Via arquivo SQL:

```bash
mysql -u user -p database_name < backup.sql
```

- Apaga e recria tabelas conforme o script
- Pode levar tempo conforme tamanho do dump

---

### **📅 Boas Práticas de Backup:**

- Fazer backups regulares e agendados
- Testar a restauração periodicamente
- Manter backups em local seguro e separado do servidor principal
- Usar compressão para economizar espaço
- Documentar procedimentos de backup e restore na equipa
- Versão e controle de backups (nome e data explícita)
- Monitorar logs de backup

---

### **📋 Backup Incremental e Point-in-Time Recovery**

- Usar backup full combinando com logs binários para restauração a ponto específico no tempo
- Importante para minimizar perda de dados entre backups