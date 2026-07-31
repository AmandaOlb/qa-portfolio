# Bug Report — Skoob (Aplicativo de Registro de Leituras)
**Testado por:** Amanda Rocha  
**Data:** Junho de 2025  
**Plataforma:** Skoob Web (pós-atualização Skeelo)  
**Tipo de teste:** Exploratório — identificação de regressões após atualização de produto  

---

## Sumário

| # | Título | Severidade | Status |
|---|--------|------------|--------|
| BUG-01 | Filtros da estante não funcionam corretamente | Alta | Aberto |
| BUG-02 | Data de leitura alterada incorretamente após migração | Alta | Aberto |
| BUG-03 | Campo de senha exibe texto em claro (sem máscara) | Alta | Aberto |
| BUG-04 | Livros desaparecem da estante do usuário | Crítica | Aberto |

---

## BUG-01 — Filtros da estante não funcionam corretamente

**Severidade:** Alta  
**Componente:** Estante / Busca  

### Descrição
Os filtros de busca disponíveis na estante do usuário não retornam resultados corretos ou não respondem à seleção realizada.

### Passos para reproduzir
1. Acessar a estante pessoal após login
2. Aplicar qualquer filtro disponível (ex: ano de leitura, status, gênero)
3. Observar os resultados exibidos

### Resultado esperado
A listagem de livros deve ser filtrada de acordo com o critério selecionado.

### Resultado obtido
Os filtros não funcionam conforme esperado — os resultados exibidos não correspondem ao critério aplicado ou a listagem permanece inalterada.

### Impacto
Usuários com estantes extensas ficam sem forma eficiente de navegar e localizar leituras registradas.

---

## BUG-02 — Data de leitura alterada incorretamente após migração

**Severidade:** Alta  
**Componente:** Registro de leituras / Migração de dados  

### Descrição
Após a atualização da plataforma, datas de leitura previamente registradas foram alteradas de forma incorreta. Livros lidos em 2025 passaram a exibir datas antigas ou incorretas.

### Passos para reproduzir
1. Acessar a estante pessoal após login
2. Localizar livros marcados como lidos em 2025
3. Verificar a data de leitura registrada

### Resultado esperado
A data de leitura deve corresponder ao registro original feito pelo usuário.

### Resultado obtido
A data exibida está incorreta. Exemplo identificado: *A Casa dos Espíritos* (lido em 2025) aparece como lido em **1970**.

### Impacto
Comprometimento da integridade dos dados históricos dos usuários — funcionalidade central da plataforma.

---

## BUG-03 — Campo de senha exibe texto em claro (sem máscara)

**Severidade:** Alta  
**Componente:** Autenticação / Login  

### Descrição
O campo de senha na tela de login perdeu sua máscara após a atualização, exibindo os caracteres digitados em texto claro por padrão.

### Passos para reproduzir
1. Acessar a tela de login do Skoob
2. Clicar no campo de senha
3. Digitar qualquer caractere

### Resultado esperado
Os caracteres devem ser ocultados por máscara (ex: `••••••••`), com opção opcional de visualização.

### Resultado obtido
Os caracteres são exibidos em texto claro, sem qualquer ocultação.

### Impacto
Risco de segurança em ambientes compartilhados ou públicos. Regressão básica de UX e segurança introduzida pela atualização.

### Observação
Trata-se de uma regressão de baixa complexidade técnica, cuja permanência após a atualização indica ausência de testes mínimos de regressão no fluxo de autenticação.

---

## BUG-04 — Livros desaparecem da estante do usuário

**Severidade:** Crítica  
**Componente:** Estante / Migração de dados  

### Descrição
Após a atualização, parte significativa dos livros registrados na estante do usuário simplesmente desapareceu. Em alguns casos, apenas as capas somem; em outros, o registro completo é perdido.

### Passos para reproduzir
1. Acessar a estante pessoal após login
2. Navegar por registros de anos anteriores
3. Comparar a quantidade de livros exibidos com registros pessoais ou memória do usuário

### Resultado esperado
Todos os livros previamente registrados devem estar disponíveis na estante, com seus dados íntegros.

### Resultado obtido
Registros históricos incompletos. Exemplo identificado: estante de 2018 exibe apenas 3 livros de um total original de aproximadamente 20.

### Impacto
Perda irreversível de dados do usuário — o histórico de leituras é a funcionalidade principal da plataforma. Impacto direto na confiança e retenção de usuários.

---

## Considerações Finais

Os bugs identificados, em especial BUG-03 e BUG-04, sugerem ausência de um processo estruturado de testes de regressão antes do lançamento da atualização. Tratam-se de falhas visíveis em fluxos críticos (autenticação e dados do usuário) que seriam detectadas em uma rodada básica de smoke testing.

A plataforma foi notificada sobre os bugs via canal oficial. Resposta recebida: itens adicionados ao backlog.

---

*Este relatório foi produzido como exercício de portfólio, com base em uso real da plataforma.*
