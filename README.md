# Plano de testes - Shortz-App

## Eduardo Duarte, Julio Cesar, Matheus Janing, Thiago Rodrigues, Caio Borba 

## 1. Riscos Identificados

| ID   | Descrição do risco                                                         | Categoria       | Prob  | Impacto | Prioridade |
|------|---------------------------------------------------------------------------|----------------|------|---------|-----------|
| R-01 | Identifica usuários com nomes únicos                                     | Funcional      | Alto | Crítico | Crítica   |
| R-02 | Upload de vídeos com 1 minuto de duração                                 | Funcional      | Baixo| Baixo   | Alta      |
| R-03 | Upload de criptográficas antes de serem armazenadas no banco             | Não funcional  | Alto | Crítico | Crítica   |
| R-04 | Formato dos campos validados no server-side                              | Funcional      | Médio| Alto    | Alto      |
| R-05 | Moderação de conteúdo +18                                                 | Funcional      | Médio| Crítico | Crítica   |
| R-06 | Exibir lista de vídeos curtidos                                          | Funcional      | Baixo| Médio   | Médio     |
| R-07 | Buscar vídeos por nome, título ou usuário                                | Funcional      | Alto | Alto    | Alto      |
| R-08 | Interface responsiva para Desktop e Mobile                               | Não funcional  | Alto | Crítico | Crítico   |
| R-09 | Contagem de visualização de vídeos quando o vídeo for reproduzido        | Funcional      | Alto | Crítico | Crítico   |
| R-10 | Emails únicos na criação de contas                                       | Funcional      | Alto | Crítico | Crítico   |