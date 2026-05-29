## Curator Skill – Busca de Cursos na Alura

### Despacho esperado (recebido do Orquestrador)
```
## DESPACHO: CURATOR
### referencia_persona
[conteúdo completo de personas/curator.md]

### tarefa
Buscar cursos na Alura que complementem as habilidades faltantes do usuário.

### perfil_usuario
[conteúdo de data/user-profile.md]

### contexto
- Habilidades faltantes: lista de habilidades que o usuário ainda não possui mas são requeridas nas vagas encontradas.
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

### Implementação lógica (pseudo‑código)
1. **Extrair habilidades faltantes** do campo `contexto`.
2. **Para cada habilidade**:
   - Construir URL de busca: `https://www.alura.com.br/busca?query={habilidade}`.
   - Tentar **firecrawl scrape** da URL (JSON output).
   - Se o comando falhar, usar **fallback**:
     - Executar `curl -s "{URL}"` via `terminal`.
     - Aplicar `grep`/`sed` para extrair título, descrição, link, duração e nível.
3. **Normalizar resultados**:
   - Cada resultado deve conter os campos acima.
   - Remover duplicados (mesmo `Link`).
   - Contar quantas habilidades cada curso cobre; ordenar decrescentemente.
   - Truncar a lista para os **15 primeiros**.
4. **Gerar resposta** no envelope de resposta definido em `dispatch.md`:
```
## RESPOSTA: CURATOR
### estado
sucesso

### resumo
Cursos encontrados que preenchem as lacunas de habilidades.

### dados
1. Título: <title>
   Descrição: <short description>
   Link: <url>
   Duração: <hours>
   Nível: <level>
2. ...

### erros
<se houver falhas>
```

### Tratamento de erros
- Se **firecrawl** falhar para uma habilidade, registrar o erro interno e continuar com as demais.
- Se **ambas as abordagens** falharem para todas as habilidades, definir `estado: erro` e incluir a mensagem de falha em `erros`.
- Sempre incluir o campo `erros` (mesmo que vazio) na resposta.

### Persistência
- Salvar o resultado bruto (JSON ou lista) em `data/curator-results.md` para auditoria.
- Atualizar `data/curator-results.md` sempre que o agente for disparado.

---

**Observação**: Este skill descreve apenas a lógica de busca e formatação. A execução real será feita pelos sub‑agentes criados via `spawn_agent` a partir do Orquestrador.