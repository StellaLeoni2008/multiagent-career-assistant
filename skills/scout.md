# Skill: Scout (Busca de Vagas)

## Despacho

### Envelope de Despacho (usado pelo Orquestrador para chamar o Scout)
```
## DESPACHO: Scout
### referencia_persona
[Conteúdo completo de personas/scout.md]

### tarefa
Buscar vagas de emprego para o perfil do usuário nos sites InfoJobs, Vagas.com e Indeed.

### perfil_usuario
[Conteúdo de data/user-profile.md]

### contexto
[Opcional: palavras‑chave adicionais, localização preferida, faixa salarial, etc.]

### saida_esperada
Lista numerada de vagas, cada item com pares chave‑valor:
- Título
- Empresa
- Local
- Link
- Salário
```

## Execução

1. **Firecrawl** – Tenta obter resultados usando o comando:
   ```sh
   firecrawl crawl "https://www.infojobs.com.br/vagas?q={cargo}" --output json
   ```
   Substitua `{cargo}` pelos cargos alvo extraídos de `Funções alvo` no `data/user-profile.md`.
   - Parseia o JSON, extrai `title`, `company`, `location`, `url`, `salary` (se disponível).
   - Repete o mesmo para `https://www.vagas.com.br/vagas?search={cargo}` e `https://www.indeed.com.br/jobs?q={cargo}`.
   - Limita a **20 vagas por site**.
2. **Fallback – Navegação Web Nativa**
   - Usa `curl` ou `wget` para baixar o HTML das páginas de busca.
   - Aplica expressões regulares (ou `grep`) para capturar título, empresa, localização e link.
   - Se ainda falhar, registra o erro.
3. **Agregação**
   - Une resultados de todos os sites.
   - Remove duplicados baseando‑se no campo `Link`.
   - Ordena por relevância (simples contagem de correspondência de palavras‑chave do cargo).
4. **Persistência**
   - Salva a lista final em `data/scout-results.md` usando o formato de lista numerada com pares chave‑valor.
   - Caso ocorram erros, inclui uma seção `Erros:` no mesmo arquivo.

## Resposta

### Envelope de Resposta (deve ser retornado pelo Scout)
```
## RESPOSTA: Scout
### estado
sucesso | erro

### resumo
[Resumo breve para o usuário]

### dados
[Lista numerada de vagas no formato especificado]

### erros
[Se estado for erro, detalhes do problema]
```

## Tratamento de Erros
- Se `firecrawl` falhar, tenta o fallback.
- Se ambos falharem, define `estado` como `erro` e preenche `erros` com a mensagem de falha.
- Sempre grava o resultado (mesmo que vazio) em `data/scout-results.md`.
