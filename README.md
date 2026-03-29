# Plano de Testes – ShortzApp 🎬

| Versão | Data | Autores |
| :--- | :--- | :--- |
| **1.1** | 27 de março de 2026 | Caio Borba, Eduardo Duarte, Julio Cesar Maçaneiro, Matheus Janing, Thiago Canal |

**Projeto:** ShortzApp – cópia descarada do tiktok.

---

## 1. Introdução
Este documento detalha o planejamento estratégico das atividades de teste para a plataforma Shortz App, um aplicativo web para o compartilhamento de vídeos curtos entre os usuários do app. A aplicação visa permitir aos usuários upload de vídeos, interação com eles através de curtidas, salvar vídeos. O objetivo deste plano de testes é garantir que funcionalidades críticas estejam cumprindo os requisitos estabelecidos de forma robusta e segura.

---

## 2. Escopo dos Testes

### As seguintes funcionalidades serão o foco das atividades de teste nesta fase do projeto:

### ✅ 2.1. Em Escopo
* **Gestão de Usuários:** Cadastro (validações de e-mail/senha), Autenticação (Login/Logout) e Edição de perfil.
* **Gestão de Vídeos:** Upload (formato, tempo, tamanho), Playlists e Visualização responsiva.
* **Interação Social:** Curtir/Descurtir e sistema de Follow.

### ❌ 2.2. Fora de Escopo
* Funcionalidades administrativas e moderação.
* Recuperação de senha via e-mail/SMS.
* Integrações externas (redes sociais).
* Busca avançada e relatórios de métricas.

---

## 3. Estratégia de Testes
A Estratégia de Testes pensada para esse programa é fundamentada no "Shift Left Testing" que antecipa as atividades de testes logo no início do projeto. Dessa forma há um maior controle de tempo e custo, facilitando a correção de erros precocemente. A pirãmide de testes foi fundamentada da seguinte forma:

1.  **Testes Unitários:** Por meio desse teste são feitas validações dos métodos e funções do projeto (menor parte do projeto), como: moderação de idade, tamanho e formato de arquivos, informações de login. Dessa forma o sistema consegue fazer a correta manipulação de dados.
2.  **Testes de Integração:** Localizados no meio da pirâmide, esses testes verificam a interação entre diferentes módulos ou serviços. Na plataforma de compartilhamento de vídeos curtos, ShortzApp, serão aplicados para garantir a comunicação eficaz entre a camada de aplicação e o banco de dados (persistência de vídeos, perfis, comentários, curtidas e visualizações), bem como a integração com o sistema de armazenamento de arquivos de vídeo.
3.  **Testes Black-Box (E2E):** No topo da pirâmide, estes testes simulam o comportamento do usuário final, validando fluxos completos da aplicação. Serão executados utilizando as técnicas de Particionamento de Equivalência, Análise de Valores-Limite e Tabelas de Decisão para garantir que o sistema atenda aos requisitos funcionais e de usabilidade, sem a necessidade de conhecimento interno do código.

Além disso, consideraremos a realização de Testes de Desempenho em uma fase posterior para avaliar a capacidade do sistema de lidar com múltiplos uploads e visualizações simultâneas de vídeos, e Testes de Segurança para identificar vulnerabilidades comuns (ex: injeção de SQL, XSS) nas funcionalidades de autenticação, upload de vídeos e manipulação de comentários.

---

## 4. Riscos e Mitigação

| Risco | Probabilidade | Impacto | Prioridade | Estratégia de Teste |
| :--- | :--- | :--- | :--- | :--- |
| Upload de arquivo malicioso | Alta | Crítico | **Crítica** | Scan de conteúdo e validação de MIME-type. |
| Senha salva sem criptografia | Média | Crítico | **Crítica** | Teste unitário no método de persistência. |
| Injeção XSS em comentários | Média | Crítico | **Crítica** | Testes de segurança e sanitização de inputs. |
| Usuário menor de idade em vídeo +18 | Média | Crítico | **Crítica** | Teste E2E de controle de acesso por perfil. |
| Upload de vídeos > 1 minuto | Média | Baixo | Baixa | Análise de Valor Limite na duração do vídeo. |
| Múltiplos usuários com o mesmo nome. | Baixa | Crítico | Alta | Aplicar Análise de Valores-Limite e Particionamento de Equivalência no tamanho e formato do arquivo. Testes unitários para validação de MIME-type e extensão. |
| Formato dos campos não validados no Server-Side | Baixo | Alto | Alta | Testes de integração e black-box com validações server-side para todos os campos de entrada (título, descrição, comentários). |
| Falha na exibição de vídeos curtos | Alta | Médio | Média | Testes de integração com o player de vídeo e testes E2E de visualização em diferentes dispositivos e redes. |
| Falha na conexão com o Banco de Dados para a busca por usuário ou título | Baixa | Alto | Alta | Testes de integração entre camada de aplicação e banco de dados para queries de busca (título e usuário). |
| Bugs na responsividade no Desktop ou Mobile | Média | Crítico | Crítica | Testes de usabilidade black-box e testes de responsividade em múltiplos navegadores e dispositivos (Desktop/Mobile). |
| Falha na contagem de visualização do vídeo ao ser acessado | Baixa | Crítico | Crítica | Testes de integração para atualização automática do contador de visualizações no  |
| Mais de um usuário autenticado no sistema com o mesmo e-mail | Baixa | Crítico | Crítica | Testes unitários e de integração na validação de e-mail único durante cadastro e login. |
| Lentidão no feed/carregamento | Alta | Alto | Alta | Testes de desempenho (carga e estresse) simulando múltiplos usuários acessando o feed e visualizando vídeos simultaneamente. |

---

## 5. Casos de Teste Planejados (Black-Box)

Esta seção detalha a aplicação das técnicas de teste black-box para modelar casos de teste manuais para funcionalidades críticas do ShortzApp. Serão apresentados exemplos de Particionamento de Equivalência, Análise de Valores-Limite e Tabelas de Decisão.

# 5.1 Funcionalidade: Validação Server-Side (Título, Descrição, Comentários)

## Particionamento de Equivalência

| Campo       | Classe Válida        | Classes Inválidas              |
|------------|---------------------|--------------------------------|
| Título     | 1 a 100 caracteres  | vazio; > 100                  |
| Descrição  | 1 a 1000 caracteres | vazio; > 1000                 |
| Comentário | 1 a 500 caracteres  | vazio; > 500; código malicioso |

## Análise de Valores-Limite

| Limite                    | Valor Mínimo | Valor Abaixo | Valor Máximo     | Valor Acima     |
|--------------------------|--------------|--------------|------------------|------------------|
| Valores de Teste (Título) | A            | vazio        | 100 caracteres   | 101 caracteres   |

## Casos de Teste Resultantes

### CT-SV-001: Entrada válida no título
- Prioridade: Alta  
- Passos: Inserir título com 50 caracteres  
- Resultado Esperado: Aceitar  

### CT-SV-002: Campo vazio
- Prioridade: Alta  
- Passos: Deixar título vazio  
- Resultado Esperado: Exibir erro  


 Funcionalidade: Controle de Acesso por Idade (+18)

## Particionamento de Equivalência

| Campo              | Classe Válida | Classes Inválidas |
|-------------------|--------------|------------------|
| Idade do Usuário  | ≥ 18 anos    | < 18 anos        |

## Análise de Valores-Limite

| Limite           | Valor Mínimo (18) | Abaixo | Valor Válido | Acima |
|------------------|-------------------|--------|--------------|--------|
| Valores de Teste | 18                | 17     | 25           | 60     |

## Casos de Teste Resultantes

### CT-ID-001: Usuário maior de idade acessa vídeo +18
- Prioridade: Crítica  
- Resultado Esperado: Acesso permitido  

### CT-ID-002: Usuário menor de idade tenta acessar
- Prioridade: Crítica  
- Resultado Esperado: Acesso bloqueado  


 Funcionalidade: Responsividade (Desktop/Mobile)

## Particionamento de Equivalência

| Campo       | Classe Válida                | Classes Inválidas        |
|------------|-----------------------------|--------------------------|
| Layout     | Adaptável (responsivo)      | Quebra de layout         |
| Dispositivo| Desktop e Mobile funcionais | Interface com erro       |

## Análise de Valores-Limite

| Limite           | Valor Ideal     | Abaixo          | Máximo                        | Acima            |
|------------------|-----------------|------------------|-------------------------------|------------------|
| Valores de Teste | Layout correto  | Pequenos erros   | Funciona em resoluções padrão | Layout quebrado  |

## Casos de Teste Resultantes

### CT-RS-001: Acesso via desktop
- Prioridade: Alta  
- Resultado Esperado: Interface correta  

### CT-RS-002: Acesso via mobile com erro de layout
- Prioridade: Crítica  
- Resultado Esperado: Detectar falha  


 Funcionalidade: Segurança – XSS em Comentários

## Particionamento de Equivalência

| Campo       | Classe Válida  | Classes Inválidas             |
|------------|---------------|-------------------------------|
| Comentário | Texto simples | Scripts e códigos maliciosos  |

## Análise de Valores-Limite

| Limite           | Entrada Válida     | Entrada Maliciosa              | Limite Aceitável      | Acima            |
|------------------|--------------------|-------------------------------|-----------------------|------------------|
| Valores de Teste | Comentário normal  | <script>alert(1)</script>     | Texto com símbolos    | Script completo  |

## Casos de Teste Resultantes

### CT-XSS-001: Comentário válido
- Prioridade: Alta  
- Resultado Esperado: Aceitar  

### CT-XSS-002: Inserção de script
- Prioridade: Crítica  
- Resultado Esperado: Rejeitar ou sanitizar

### 5.2. Técnica: Tabela de Decisão

---

### 5.2.1 Funcionalidade: Validação Server-Side (Título, Descrição, Comentários)

**Regra:** Os dados só são aceitos se todos os campos forem válidos.

### Tabela de Decisão

| Condições/Ações        | R1 | R2 | R3 | R4 | R5 | R6 | R7 | R8 |
|-----------------------|----|----|----|----|----|----|----|----|
| Título válido?        | S  | S  | S  | S  | N  | N  | N  | N  |
| Descrição válida?     | S  | S  | N  | N  | S  | S  | N  | N  |
| Comentário válido?    | S  | N  | S  | N  | S  | N  | S  | N  |
| **Ação: Aceitar**     | S  | N  | N  | N  | N  | N  | N  | N  |
| **Ação: Exibir Erro** | N  | S  | S  | S  | S  | S  | S  | S  |

---

### Casos de Teste Resultantes

#### CT-SV-001 (R1): Dados válidos
- **Prioridade:** Alta  
- **Pré-condições:** Sistema disponível  
- **Passos:** Inserir título, descrição e comentário válidos  
- **Resultado Esperado:** Dados aceitos  

---

#### CT-SV-002 (R2): Comentário inválido
- **Prioridade:** Alta  
- **Passos:** Inserir comentário inválido  
- **Resultado Esperado:** Exibir erro  

---

#### CT-SV-003 (R5): Título inválido
- **Prioridade:** Alta  
- **Passos:** Inserir título inválido  
- **Resultado Esperado:** Exibir erro  

---

### 5.2.2. Funcionalidade: Controle de Acesso por Idade (+18)

**Regra:** Apenas usuários com 18 anos ou mais podem acessar conteúdo +18.

### Tabela de Decisão

| Condições/Ações            | R1 | R2 |
|---------------------------|----|----|
| Usuário ≥ 18?             | S  | N  |
| **Ação: Permitir Acesso** | S  | N  |
| **Ação: Bloquear Acesso** | N  | S  |

---

### Casos de Teste Resultantes

#### CT-ID-001 (R1): Usuário maior de idade
- **Prioridade:** Crítica  
- **Passos:** Acessar conteúdo +18  
- **Resultado Esperado:** Acesso permitido  

---

#### CT-ID-002 (R2): Usuário menor de idade
- **Prioridade:** Crítica  
- **Passos:** Tentar acessar conteúdo +18  
- **Resultado Esperado:** Acesso bloqueado  

---

### 5.2.3. Funcionalidade: Responsividade (Desktop/Mobile)

**Regra:** O sistema deve funcionar corretamente em diferentes dispositivos.

### Tabela de Decisão

| Condições/Ações             | R1 | R2 | R3 | R4 |
|------------------------------|----|----|----|----|
| Layout responsivo?           | S  | S  | N  | N  |
| Funciona no dispositivo?     | S  | N  | S  | N  |
| **Ação: Interface Correta**  | S  | N  | N  | N  |
| **Ação: Exibir Erro**        | N  | S  | S  | S  |

---

### Casos de Teste Resultantes

#### CT-RS-001 (R1): Layout correto
- **Prioridade:** Alta  
- **Passos:** Acessar sistema em desktop/mobile  
- **Resultado Esperado:** Interface correta  

---

#### CT-RS-002 (R4): Layout quebrado
- **Prioridade:** Crítica  
- **Resultado Esperado:** Identificar erro de responsividade  

---

### 5.2.4. Funcionalidade: Segurança – XSS em Comentários

**Regra:** Scripts maliciosos devem ser bloqueados ou sanitizados.

### Tabela de Decisão

| Condições/Ações              | R1 | R2 |
|-------------------------------|----|----|
| Entrada contém script?        | N  | S  |
| **Ação: Aceitar Comentário**  | S  | N  |
| **Ação: Rejeitar/Sanitizar**  | N  | S  |

---

### Casos de Teste Resultantes

#### CT-XSS-001 (R1): Comentário válido
- **Prioridade:** Alta  
- **Resultado Esperado:** Aceitar  

---

#### CT-XSS-002 (R2): Script malicioso
- **Prioridade:** Crítica  
- **Resultado Esperado:** Rejeitar ou sanitizar  

---
---

# 6. Critérios de Aceitação

## 6.1. Critérios de Entrada

As atividades de teste das funcionalidades do **ShortzApp** somente serão iniciadas quando todas as seguintes condições forem atendidas:

- Todos os requisitos funcionais e não funcionais relacionados às funcionalidades de **Gestão de Usuários**, **Gestão de Vídeos** e **Interação Social** estiverem finalizados, documentados e aprovados pela equipe de produto.

- O ambiente de testes (servidor de aplicação, banco de dados e armazenamento de arquivos de vídeo) estiver devidamente configurado, estável e acessível, garantindo compatibilidade com o ambiente de produção.

- As builds do **ShortzApp** contendo as funcionalidades a serem testadas estiverem disponíveis e corretamente implantadas no ambiente de testes.

- A equipe de testes possuir acesso às ferramentas necessárias para execução e acompanhamento dos testes, tais como:
  - Gerenciador de casos de teste  
  - Ferramentas de monitoramento de logs  
  - Sistema de registro de defeitos  

---

## 6.2. Critérios de Saída

O ciclo de testes será considerado concluído, e as funcionalidades estarão aptas para implantação em produção, quando todos os critérios abaixo forem atendidos:

- 100% dos casos de teste classificados com prioridade **Crítica** e **Alta** forem executados com sucesso.

- Todos os defeitos classificados com severidade **Crítica** e **Alta** forem corrigidos, re-testados, validados e encerrados.

- A cobertura de requisitos das funcionalidades em escopo atingir no mínimo **90%**.

- Não existirem defeitos de severidade **Média** ou **Baixa** que comprometam:
  - A experiência do usuário (ex: falhas no feed, responsividade ou interação)
  - A integridade dos dados (ex: inconsistência em curtidas, comentários ou usuários)

- Os resultados dos testes de desempenho indicarem que o sistema suporta adequadamente a carga esperada, especialmente nas funcionalidades de:
  - Upload de vídeos  
  - Reprodução de vídeos  
  - Carregamento do feed  

---

## 6.3. Critérios de Suspensão

As atividades de teste poderão ser temporariamente suspensas caso ocorra qualquer uma das seguintes situações:

- Mais de **20% dos casos de teste de prioridade Alta** apresentarem falha na primeira execução, indicando instabilidade significativa da build.

- O ambiente de testes se tornar indisponível ou instável por um período superior a **4 horas consecutivas**, impossibilitando a continuidade dos testes.

- Uma funcionalidade crítica do sistema (como login, upload de vídeos ou reprodução de vídeos) apresentar um **defeito bloqueador**, impedindo o progresso dos testes em funcionalidades dependentes.

- Ocorrerem mudanças significativas nos requisitos das funcionalidades em teste sem aviso prévio, tornando os casos de teste atuais inválidos.

---
