### **🔥 O que são Triggers?**

#### **Definição:**

> Triggers são procedimentos especiais que executam automaticamente (são "disparados") em resposta a eventos específicos numa tabela ou vista. Não podem ser chamados diretamente - apenas executam quando o evento associado ocorre.

#### **Características dos Triggers:**

```text
🔄 Automáticos - Executam sem intervenção manual
⚡ Transparentes - Aplicação não precisa saber que existem
🛡️ Integridade - Garantem regras de negócio
📊 Auditoria - Registam mudanças automaticamente
⚠️ Invisíveis - Difíceis de debuggar e rastrear
🐌 Performance - Podem tornar operações mais lentas
```

#### **Tipos de Triggers:**

```text
⏰ Timing:
   - BEFORE: Antes da operação (validação, transformação)
   - AFTER: Depois da operação (log, notificação)
   
🎯 Eventos:
   - INSERT: Quando novos registos são inseridos
   - UPDATE: Quando registos são modificados
   - DELETE: Quando registos são removidos
```

### **🆕 Criar Triggers:**

#### **Sintaxe Básica:**

```sql
CREATE TRIGGER trigger_name
{BEFORE | AFTER} {INSERT | UPDATE | DELETE}
ON table_name
FOR EACH ROW
BEGIN
    -- Código do trigger
END;
```

#### **Trigger BEFORE INSERT:**

```sql
-- Validação antes de inserir
DELIMITER //
CREATE TRIGGER validate_customer_before_insert
BEFORE INSERT ON customers
FOR EACH ROW
BEGIN
    -- Validar email
    IF NEW.email NOT LIKE '%@%' THEN
        SIGNAL SQLSTATE '45000' 
        SET MESSAGE_TEXT = 'Email inválido - deve conter @';
    END IF;
    
    -- Normalizar dados
    SET NEW.email = LOWER(TRIM(NEW.email));
    SET NEW.first_name = TRIM(NEW.first_name);
    SET NEW.last_name = TRIM(NEW.last_name);
    
    -- Definir valores padrão
    IF NEW.status IS NULL THEN
        SET NEW.status = 'active';
    END IF;
    
    -- Gerar customer number se não fornecido
    IF NEW.customer_number IS NULL THEN
        SET NEW.customer_number = CONCAT('CUST', LPAD((SELECT COALESCE(MAX(id), 0) + 1 FROM customers), 6, '0'));
    END IF;
END //
DELIMITER ;

-- Teste do trigger
INSERT INTO customers (first_name, last_name, email)
VALUES ('João', ' Silva ', 'JOAO@EMAIL.COM ');

-- Resultado: email = 'joao@email.com', nomes trimmed, customer_number gerado
```

#### **Trigger AFTER INSERT:**

```sql
-- Log de auditoria após inserção
DELIMITER //
CREATE TRIGGER audit_customer_insert
AFTER INSERT ON customers
FOR EACH ROW
BEGIN
    INSERT INTO audit_log (
        table_name,
        operation,
        record_id,
        new_values,
        changed_by,
        changed_at
    ) VALUES (
        'customers',
        'INSERT',
        NEW.id,
        CONCAT('name: ', NEW.first_name, ' ', NEW.last_name, 
               ', email: ', NEW.email,
               ', status: ', NEW.status),
        USER(),
        NOW()
    );
    
    -- Atualizar contador global
    UPDATE system_stats 
    SET customer_count = customer_count + 1,
        last_updated = NOW()
    WHERE stat_name = 'total_customers';
    
    -- Enviar notificação de boas-vindas
    INSERT INTO notification_queue (
        notification_type,
        recipient_email,
        subject,
        message,
        priority,
        created_at
    ) VALUES (
        'WELCOME_EMAIL',
        NEW.email,
        'Bem-vindo!',
        CONCAT('Olá ', NEW.first_name, ', obrigado por se registar!'),
        'NORMAL',
        NOW()
    );
END //
DELIMITER ;
```

### **🔧 Triggers de UPDATE:**

#### **BEFORE UPDATE - Validação e Transformação:**

```sql
DELIMITER //
CREATE TRIGGER validate_product_before_update
BEFORE UPDATE ON products
FOR EACH ROW
BEGIN
    -- Verificar se preço não diminui mais de 50%
    IF NEW.price < OLD.price * 0.5 THEN
        SIGNAL SQLSTATE '45000' 
        SET MESSAGE_TEXT = 'Preço não pode diminuir mais de 50% de uma vez';
    END IF;
    
    -- Atualizar timestamp automaticamente
    SET NEW.updated_at = NOW();
    
    -- Se stock mudou, atualizar data da última movimentação
    IF NEW.stock_quantity != OLD.stock_quantity THEN
        SET NEW.last_stock_movement = NOW();
    END IF;
    
    -- Calcular margem automaticamente se cost_price mudou
    IF NEW.cost_price != OLD.cost_price THEN
        SET NEW.profit_margin = ((NEW.price - NEW.cost_price) / NEW.price) * 100;
    END IF;
    
    -- Validar dados críticos
    IF NEW.price <= 0 THEN
        SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Preço deve ser positivo';
    END IF;
    
    IF NEW.stock_quantity < 0 THEN
        SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Stock não pode
```