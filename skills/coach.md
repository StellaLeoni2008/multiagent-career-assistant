## Skill – Coach (Simulação de Entrevista)

### Objetivo

Gerar e conduzir uma entrevista simulada baseada no perfil do usuário (`data/user-profile.md`). Produzir perguntas técnicas, comportamentais e situacionais, adaptar dinamicamente as próximas perguntas com base nas respostas anteriores e registrar o histórico em `data/coach-history.md`.

### Entrada esperada (Envelope de Despacho)
```
## DESPACHO: COACH
### referencia_persona
[conteúdo completo de personas/coach.md]

### tarefa
Simular entrevista para a vaga especificada no perfil do usuário.

### perfil_usuario
[conteúdo de data/user-profile.md]

### contexto
- Área de atuação: extraída do perfil.
- Nível da vaga (estágio, júnior, pleno, sênior): extraído do perfil.
- Cargo desejado: inferido a partir das "Funções alvo" do perfil.
- Histórico da entrevista anterior (se existir) em data/coach-history.md.

### saida_esperada
Lista numerada de perguntas com:
1. Pergunta
2. Resposta (campo a ser preenchido pelo usuário na próxima interação)
Formato: listas numeradas com pares chave‑valor, sem tabelas markdown.
```

### Procedimento interno
1. **Carregar perfil** – usar `read_file` para obter `data/user-profile.md`.
2. **Determinar contexto** – extrair `Área de atuação`, `Nível da vaga` e `Funções alvo`.
3. **Banco de perguntas** – manter um dicionário interno (hard‑coded) de perguntas organizadas por:
   - Área (Frontend, Backend, Dados, etc.)
   - Nível (estágio, júnior, pleno, sênior)
   - Tipo (técnica, comportamental, situacional)
4. **Seleção inicial** – escolher aleatoriamente:
   - 2‑3 perguntas técnicas
   - 2 perguntas comportamentais
   - 1‑2 situacionais
   Personalizar inserindo o nome da vaga/cargo nas questões.
5. **Persistir histórico** – se `data/coach-history.md` existir, ler e incluir as perguntas já feitas; caso contrário, iniciar novo arquivo.
6. **Gerar resposta** – montar o envelope de resposta (`## RESPOSTA: COACH`) contendo:
   - `estado` = `sucesso`
   - `resumo` breve da entrevista iniciada
   - `dados` = lista numerada de perguntas (sem respostas ainda)
   - `erros` vazio
7. **Salvar histórico** – gravar as perguntas geradas em `data/coach-history.md` usando `edit_file` (append). Cada entrada deve seguir o formato:
   ```markdown
   1. Pergunta: "..."
      Resposta: ""
   ```
8. **Adaptação dinâmica** – nas chamadas subsequentes (quando o usuário responder), ler `coach-history.md`, analisar a última resposta (palavras‑chave simples) e selecionar a próxima pergunta que aprofunde o ponto mencionado ou cubra uma lacuna ainda não abordada.
9. **Encerramento** – após um número predefinido de rodadas (ex.: 6) ou quando não houver mais perguntas, gerar um resumo de pontos fortes e áreas de melhoria e incluir sugestões de estudo (pode delegar ao Curator).

### Saída esperada (Envelope de Resposta)
```
## RESPOSTA: COACH
### estado
sucesso

### resumo
Entrevista simulada iniciada para a vaga de <Cargo>. Serão feitas X perguntas.

### dados
1. Pergunta: "..."
2. Pergunta: "..."
...
### erros
```

Se ocorrer algum erro (ex.: falta de `user-profile.md`), preencher `estado` com `erro` e descrever o problema em `erros`.
