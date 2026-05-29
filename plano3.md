# Plano 3 – Agente Curator (Busca de Cursos)

## Visão Geral

Desenvolver o agente **Curator** responsável por buscar cursos na **Alura** que preencham as lacunas de habilidades identificadas a partir do quiz do usuário e das vagas encontradas pelo agente Scout. O Curator será acionado pelo Orquestrador quando o usuário escolher a opção **B** no menu.

## Ferramentas

1. **firecrawl** – Utilizado para navegar e extrair conteúdo das páginas de cursos da Alura (https://www.alura.com.br).  
2. **Acesso Web nativo** – Caso o `firecrawl` falhe, o agente deve recorrer ao mecanismo de navegação web interno do Zed (via `spawn_agent` ou `terminal` com `curl`/`wget`).

## Estrutura de Skills

Criar o arquivo `skills/curator.md` contendo:

- **Envelope de despacho** para sub‑agentes de extração de cursos.  
- **Formato de saída esperado** (listas numeradas com pares chave‑valor: `Título`, `Descrição`, `Link`, `Duração`, `Nível`).  
- **Tratamento de erros**: registrar o erro em campo `erros` e abortar a operação se ambos os métodos falharem.

## Passos de Implementação

### 1. Definir Prompt de Despacho
```
## DESPACHO: CURATOR
### referencia_persona
[Conteúdo completo de personas/curator.md]

### tarefa
Buscar cursos na Alura que complementem as habilidades faltantes do usuário.

### perfil_usuario
[Conteúdo de data/user-profile.md]

### contexto
- Habilidades faltantes: (lista extraída das lacunas entre habilidades atuais e requisitos das vagas encontradas)
- Prioridade: cursos que atendam ao maior número de lacunas primeiro.
- Limite: no máximo 15 cursos relevantes.

### saida_esperada
Lista numerada de cursos com:
1. Título
2. Descrição curta
3. Link
4. Duração (horas)
5. Nível (Iniciante, Intermediário, Avançado)
```

### 2. Implementar Busca com firecrawl
- **Endpoint**: `firecrawl scrape "https://www.alura.com.br/busca?query={skill}" --output json`.
- Para cada habilidade faltante, executar o comando acima.
- Parsear o JSON retornado, extrair:
  - `title` → **Título**
  - `description` (primeiros 150 caracteres) → **Descrição**
  - `url` → **Link**
  - `duration` (se disponível) → **Duração**
  - `level` (se disponível) → **Nível**
- Consolidar resultados, remover duplicados (mesmo `Link`).

### 3. Fallback – Navegação Web Nativa
- Utilizar `curl` para obter o HTML da página de busca da Alura.
- Aplicar expressões regulares (`grep`, `sed`) para capturar os mesmos campos acima.
- Se ainda falhar, registrar erro em `erros`.

### 4. Agregação e Prioritização
- Ordenar cursos por número de habilidades que cobrem (um curso pode aparecer em buscas de várias habilidades; contar ocorrências).
- Limitar a 15 cursos, priorizando os que cobrem mais lacunas.

### 5. Persistência
- Salvar o resultado em `data/curator-results.md` usando a estrutura de lista numerada.
- Exemplo de saída:

```markdown
1. Título: **Fundamentos de Python**
   Descrição: Aprenda os conceitos básicos da linguagem Python, sintaxe, tipos de dados e estruturas de controle.
   Link: https://www.alura.com.br/curso-python-fundamentos
   Duração: 12h
   Nível: Iniciante

2. Título: **SQL para Análise de Dados**
   Descrição: Domine consultas SQL para extrair, filtrar e transformar dados em bancos relacionais.
   Link: https://www.alura.com.br/curso-sql-analise-dados
   Duração: 8h
   Nível: Intermediário
```

- Atualizar campo `erros` caso haja falhas.

### 6. Integração com o Orquestrador
- Quando o usuário escolher **B** no menu, o Orquestrador deve:
  1. `spawn_agent` com o envelope acima.
  2. Receber a **RESPOSTA** do Curator.
  3. Exibir ao usuário a lista de cursos formatada.
  4. Voltar ao menu principal.

## Próximos Passos

1. **Criar a persona** `personas/curator.md` descrevendo o comportamento, limitações e estilo de comunicação do agente Curator.
2. **Implementar a skill** `skills/curator.md` seguindo o fluxo descrito.
3. **Atualizar** `skills/dispatch.md` para incluir a rota do agente Curator (opção **B**).
4. **Testar** a busca de cursos com firecrawl e validar o fallback.
5. **Documentar** quaisquer dependências adicionais (ex.: chave de API do Firecrawl) em `data/.env` ou similar.

---

Este plano complementa o que já foi definido nos arquivos `plano.md` e `plano2.md`, fechando o ciclo de recomendação: **vagas → lacunas de habilidades → cursos**.