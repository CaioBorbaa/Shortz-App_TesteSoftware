# Plano de Testes – ShortzApp 🎬

| Versão | Data | Autores |
| :--- | :--- | :--- |
| **1.1** | 27 de março de 2026 | Caio Borba, Eduardo Duarte, Julio Cesar Maçaneiro, Matheus Janing, Thiago Canal |

**Projeto:** ShortzApp – Plataforma de vídeos curtos.

---

## 1. Introdução
Este documento detalha o planejamento estratégico das atividades de teste para a plataforma **ShortzApp**, um aplicativo web para o compartilhamento de vídeos curtos. O objetivo é garantir que as funcionalidades críticas cumpram os requisitos estabelecidos de forma robusta e segura.

---

## 2. Escopo dos Testes

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
Baseada no conceito de **Shift Left Testing**, antecipando testes desde o início do ciclo de vida.

1.  **Testes Unitários:** Validação de métodos e funções (ex: moderação de idade, tipos de arquivo).
2.  **Testes de Integração:** Comunicação entre a aplicação, banco de dados (MariaDB/PHP) e sistemas de arquivos.
3.  **Testes Black-Box (E2E):** Simulação do usuário final usando Particionamento de Equivalência e Tabelas de Decisão.

---

## 4. Riscos e Mitigação

| Risco | Probabilidade | Impacto | Prioridade | Estratégia de Teste |
| :--- | :--- | :--- | :--- | :--- |
| Upload de arquivo malicioso | Alta | Crítico | **Crítica** | Scan de conteúdo e validação de MIME-type. |
| Senha salva sem criptografia | Média | Crítico | **Crítica** | Teste unitário no método de persistência. |
| Injeção XSS em comentários | Média | Crítico | **Crítica** | Testes de segurança e sanitização de inputs. |
| Usuário menor de idade em vídeo +18 | Média | Crítico | **Crítica** | Teste E2E de controle de acesso por perfil. |
| Upload de vídeos > 1 minuto | Média | Baixo | Baixa | Análise de Valor Limite na duração do vídeo. |

---

## 5. Casos de Teste Planejados (Black-Box)

### 5.1. Particionamento de Equivalência e Valor Limite
**Funcionalidade:** Cadastro de Usuário - Campo Senha (8 a 20 caracteres)

| Limite | Valor de Teste | Resultado Esperado |
| :--- | :--- | :--- |
| Valor Abaixo (7) | `Abc@123` | Erro: Mínimo 8 caracteres |
| Valor Mínimo (8) | `Abc@1234` | Sucesso |
| Valor Máximo (20) | `SenhaForte@2026_Teste` | Sucesso |
| Acima do Máximo (21)| `SenhaMuitoLongaExcede21` | Erro: Máximo 20 caracteres |

---

### 5.2. Tabela de Decisão
**Funcionalidade:** Criação de Álbum/Playlist

| Condições | R1 | R2 | R3 | R4 |
| :--- | :---: | :---: | :---: | :---: |
| Usuário Logado? | S | S | S | N |
| Nome do Álbum Único? | S | S | N | S |
| Nº Fotos/Vídeos <= 50? | S | N | S | S |
| **Ação: Criar Álbum** | **SIM** | NÃO | NÃO | NÃO |

---

## 6. Critérios de Aceitação

### 🚩 6.1. Critérios de Entrada
* Requisitos aprovados pela equipe de produto.
* Ambiente de teste (PHP/MariaDB) configurado e estável.
* Builds implantadas com sucesso no ambiente de QA.

### 🏁 6.2. Critérios de Saída
* 100% dos testes de prioridade **Crítica/Alta** aprovados.
* Cobertura de requisitos mínima de **90%**.
* Sem bugs impeditivos na experiência do usuário.

### ⚠️ 6.3. Critérios de Suspensão
* Falha em mais de 20% dos casos de teste de prioridade Alta.
* Instabilidade no ambiente por mais de 4 horas.
* Defeito bloqueador em funcionalidade central (ex: Login).
