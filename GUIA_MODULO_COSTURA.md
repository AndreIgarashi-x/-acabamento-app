# 🧵 GUIA: Módulo Costura

## ✅ O Que Foi Criado

Um novo módulo **Costura** idêntico ao módulo Acabamento, mas com:
- **Cor azul** para identificação visual
- **Processos específicos de costura**
- **Mesmas funcionalidades** (Timer, Dashboard, Admin)

---

## 📋 Estrutura Criada

### **Backend**
- ✅ `backend/migrations/004_add_costura_module.sql`
  - Módulo Costura criado
  - 10 processos de costura cadastrados

### **Frontend**
- ✅ `frontend/src/layouts/CosturaLayout.jsx` (navbar azul)
- ✅ `frontend/src/routes/CosturaRoutes.jsx` (rotas do módulo)
- ✅ `frontend/src/App.jsx` atualizado (rota `/costura/*`)
- ✅ `frontend/src/pages/ModuleSelector.jsx` atualizado (card azul Costura)

---

## 🎨 Cores dos Módulos

```
┌──────────────────────────────────────┐
│  ✂️ Acabamento  → VERDE   (green)   │
│  🧵 Costura     → AZUL    (blue)    │
│  🖨️ Estampas   → ROXO    (purple)  │
└──────────────────────────────────────┘
```

---

## 🚀 PASSO 1: Executar Migration 004

### **1.1. Abrir Supabase SQL Editor**
1. Acesse: https://supabase.com/dashboard
2. Selecione seu projeto **acabamento-app**
3. Clique em **SQL Editor**

### **1.2. Copiar e Executar a Migration**

```sql
-- ==========================================================
-- MIGRATION 004: Adicionar Módulo Costura
-- Data: 2025-11-04
-- Descrição: Criar módulo Costura (idêntico ao Acabamento)
-- ==========================================================

BEGIN;

-- ===========================
-- 1. CRIAR MÓDULO COSTURA
-- ===========================
INSERT INTO modulos (codigo, nome, descricao, ativo)
VALUES ('costura', 'Costura', 'Gestão de OFs e processos de costura', true)
ON CONFLICT (codigo) DO NOTHING;

-- ===========================
-- 2. CRIAR PROCESSOS DE COSTURA
-- ===========================
DO $$
DECLARE
  v_modulo_id INTEGER;
BEGIN
  -- Obter ID do módulo Costura
  SELECT id INTO v_modulo_id FROM modulos WHERE codigo = 'costura';

  -- Inserir processos de costura
  INSERT INTO processes (nome, descricao, modulo_id, ordem, ativo)
  VALUES
    ('Corte', 'Corte de tecidos e materiais', v_modulo_id, 1, true),
    ('Preparação', 'Preparação das peças para costura', v_modulo_id, 2, true),
    ('Costura Reta', 'Costura reta básica', v_modulo_id, 3, true),
    ('Costura Overloque', 'Acabamento com overloque', v_modulo_id, 4, true),
    ('Galoneira', 'Acabamento com galoneira', v_modulo_id, 5, true),
    ('Pregar Botão', 'Colocação de botões', v_modulo_id, 6, true),
    ('Caseado', 'Fazer casas para botões', v_modulo_id, 7, true),
    ('Travete', 'Reforço com travetes', v_modulo_id, 8, true),
    ('Montagem', 'Montagem final da peça', v_modulo_id, 9, true),
    ('Revisão', 'Revisão final da peça costurada', v_modulo_id, 10, true)
  ON CONFLICT DO NOTHING;

  RAISE NOTICE '✅ Processos de Costura criados!';
END $$;

-- ===========================
-- 3. VERIFICAÇÃO
-- ===========================
DO $$
DECLARE
  v_modulo_id INTEGER;
  v_total_processos INTEGER;
BEGIN
  SELECT id INTO v_modulo_id FROM modulos WHERE codigo = 'costura';

  SELECT COUNT(*) INTO v_total_processos
  FROM processes
  WHERE modulo_id = v_modulo_id;

  RAISE NOTICE '';
  RAISE NOTICE '========================================';
  RAISE NOTICE '✅ Migration 004 concluída com sucesso!';
  RAISE NOTICE '========================================';
  RAISE NOTICE '🧵 Módulo Costura ID: %', v_modulo_id;
  RAISE NOTICE '📋 Total de processos: %', v_total_processos;
  RAISE NOTICE '========================================';
  RAISE NOTICE '';
END $$;

COMMIT;
```

### **1.3. Verificar Resultado**

Você deve ver:
```
✅ Migration 004 concluída com sucesso!
🧵 Módulo Costura ID: 3
📋 Total de processos: 10
```

---

## 👤 PASSO 2: Dar Permissão ao Usuário

### **2.1. Executar Query no Supabase**

```sql
-- Dar acesso ao módulo Costura para seu usuário
UPDATE users
SET modulos_permitidos = ARRAY['acabamento', 'costura', 'estampas']
WHERE matricula = 'ANDRE001';  -- ✏️ Substitua pela sua matrícula
```

### **2.2. Verificar Permissões**

```sql
-- Ver permissões do usuário
SELECT
  nome,
  matricula,
  perfil,
  modulos_permitidos
FROM users
WHERE matricula = 'ANDRE001';  -- ✏️ Substitua pela sua matrícula
```

**Resultado esperado:**
```
nome         | matricula | perfil | modulos_permitidos
-------------|-----------|--------|---------------------------
Andre Jesus  | ANDRE001  | admin  | {acabamento,costura,estampas}
```

---

## 🧪 PASSO 3: Testar o Módulo Costura

### **3.1. Fazer Logout e Login Novamente**

1. No navegador, clique em **Sair**
2. Faça **login** novamente com sua matrícula e PIN
3. Você deve ver **3 cards** na tela de seleção:
   - ✂️ **Acabamento** (verde)
   - 🧵 **Costura** (azul) ← **NOVO!**
   - 🖨️ **Estampas** (roxo)

### **3.2. Acessar o Módulo Costura**

1. Clique no card **🧵 Costura** (azul)
2. ✅ Você deve ver a navbar **AZUL**
3. ✅ O título deve ser **"Costura"**
4. ✅ Deve mostrar o Timer (mesma tela do Acabamento)

### **3.3. Testar Navegação**

**No menu azul, clique em:**

1. **Dashboard**
   - ✅ Deve abrir o Dashboard (com estatísticas)
   - ✅ Navbar continua AZUL

2. **Timer**
   - ✅ Deve abrir o Timer
   - ✅ Navbar continua AZUL
   - ✅ Ao selecionar processos, deve mostrar os **10 processos de Costura**:
     1. Corte
     2. Preparação
     3. Costura Reta
     4. Costura Overloque
     5. Galoneira
     6. Pregar Botão
     7. Caseado
     8. Travete
     9. Montagem
     10. Revisão

3. **Admin** (se você for admin/gestor)
   - ✅ Deve abrir a tela de Admin
   - ✅ Navbar continua AZUL

### **3.4. Voltar e Trocar de Módulo**

1. Clique no botão **"← Voltar"** (canto superior esquerdo)
2. ✅ Deve voltar para a tela de seleção (3 cards)
3. Clique em **Acabamento** (verde)
4. ✅ Navbar deve ficar VERDE
5. ✅ Processos devem ser de Acabamento
6. Volte e entre em **Costura** novamente
7. ✅ Navbar deve ficar AZUL
8. ✅ Processos devem ser de Costura

---

## 📊 PASSO 4: Verificar no Banco de Dados

### **4.1. Ver Todos os Módulos**

```sql
SELECT
  id,
  codigo,
  nome,
  descricao,
  ativo
FROM modulos
ORDER BY id;
```

**Resultado esperado:**
```
id | codigo     | nome        | descricao                              | ativo
---|------------|-------------|----------------------------------------|------
1  | acabamento | Acabamento  | Gestão de OFs e processos de acaba...  | true
2  | estampas   | Estampas    | Gestão de OFs e processos de estam...  | true
3  | costura    | Costura     | Gestão de OFs e processos de costura   | true
```

### **4.2. Ver Processos de Costura**

```sql
SELECT
  p.id,
  p.nome,
  p.descricao,
  p.ordem,
  m.nome AS modulo
FROM processes p
JOIN modulos m ON m.id = p.modulo_id
WHERE m.codigo = 'costura'
ORDER BY p.ordem;
```

**Resultado esperado:**
```
id  | nome              | descricao                     | ordem | modulo
----|-------------------|-------------------------------|-------|--------
... | Corte             | Corte de tecidos e materiais  | 1     | Costura
... | Preparação        | Preparação das peças...       | 2     | Costura
... | Costura Reta      | Costura reta básica           | 3     | Costura
... | Costura Overloque | Acabamento com overloque      | 4     | Costura
... | Galoneira         | Acabamento com galoneira      | 5     | Costura
... | Pregar Botão      | Colocação de botões           | 6     | Costura
... | Caseado           | Fazer casas para botões       | 7     | Costura
... | Travete           | Reforço com travetes          | 8     | Costura
... | Montagem          | Montagem final da peça        | 9     | Costura
... | Revisão           | Revisão final da peça...      | 10    | Costura
```

### **4.3. Contar Processos por Módulo**

```sql
SELECT
  m.nome AS modulo,
  COUNT(p.id) AS total_processos
FROM modulos m
LEFT JOIN processes p ON p.modulo_id = m.id
GROUP BY m.id, m.nome
ORDER BY m.nome;
```

**Resultado esperado:**
```
modulo      | total_processos
------------|----------------
Acabamento  | 13
Costura     | 10
Estampas    | 0
```

---

## 🎯 RESULTADO FINAL

### **Tela de Seleção de Módulos**

```
┌──────────────────┐  ┌──────────────────┐  ┌──────────────────┐
│  ✂️ Acabamento   │  │   🧵 Costura     │  │  🖨️ Estampas    │
│                  │  │                  │  │                  │
│   (VERDE)        │  │   (AZUL)         │  │   (ROXO)         │
│                  │  │                  │  │                  │
│  [Acessar →]     │  │  [Acessar →]     │  │  [Acessar →]     │
└──────────────────┘  └──────────────────┘  └──────────────────┘
```

### **Navegação Costura (Azul)**

```
┌────────────────────────────────────────────────────┐
│  ← │ 🏢 DCJ - Costura │ 📊 Dashboard │ ⏱️ Timer │ ⚙️ Admin │ 🚪 Sair  │  ← NAVBAR AZUL
└────────────────────────────────────────────────────┘
```

---

## ✅ CHECKLIST DE VERIFICAÇÃO

- [ ] Migration 004 executada no Supabase
- [ ] Módulo Costura criado no banco
- [ ] 10 processos de Costura criados
- [ ] Usuário com permissão `costura` no array `modulos_permitidos`
- [ ] Logout e login feitos
- [ ] 3 cards aparecem na tela de seleção
- [ ] Card Costura é AZUL
- [ ] Ao clicar em Costura, navbar fica AZUL
- [ ] Timer mostra processos de Costura
- [ ] Dashboard funciona
- [ ] Admin funciona
- [ ] Botão "← Voltar" funciona
- [ ] Trocar entre módulos funciona corretamente

---

## 🆘 TROUBLESHOOTING

### **Problema: Card Costura não aparece**

**Causa:** Usuário sem permissão

**Solução:**
```sql
-- Adicionar permissão Costura
UPDATE users
SET modulos_permitidos = ARRAY['acabamento', 'costura', 'estampas']
WHERE matricula = 'SUA_MATRICULA';
```

Depois faça **logout e login** novamente.

---

### **Problema: Navbar não fica azul**

**Causa:** Cache do navegador

**Solução:**
1. Pressione **Ctrl+Shift+R** (hard refresh)
2. Ou limpe o cache: **F12** → Application → Clear Storage → Clear site data

---

### **Problema: Processos não aparecem no Timer**

**Causa:** Migration não executou corretamente

**Solução:**
```sql
-- Verificar se processos existem
SELECT COUNT(*) FROM processes
WHERE modulo_id = (SELECT id FROM modulos WHERE codigo = 'costura');

-- Se retornar 0, executar a migration novamente
```

---

## 🎉 PRONTO!

Agora você tem **3 módulos independentes**:

- ✂️ **Acabamento** (verde) - Processos de acabamento
- 🧵 **Costura** (azul) - Processos de costura
- 🖨️ **Estampas** (roxo) - Bordado, DTF e Patch

Cada um com suas próprias:
- Cores de identificação
- Processos específicos
- Navegação independente
- OFs separadas (quando executar migration 003)

**Sistema multi-módulo funcionando perfeitamente! 🚀**
