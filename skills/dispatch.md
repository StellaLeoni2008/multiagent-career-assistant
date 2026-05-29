## Protocolo de despacho e handoff

### Roteamento de agentes
- **A – Scout**: Busca de vagas
- **B – Curator**: Busca de cursos
- **C – Coach**: Simulação de entrevista
- **D – Orquestrador**: Lida com o quiz e menu

### Envelope de Despacho (Orquestrador → sub‑agente)
```
## DESPACHO: [NOME_DO_AGENTE]
### referencia_persona
[conteúdo completo de personas/<nome_do_agente_minusculo>.md]

### tarefa
[uma frase descrevendo a tarefa]

### perfil_usuario
[conteúdo de data/user-profile.md]

### contexto
[contexto específico, ex.: sites de busca, palavras‑chave]

### saida_esperada
[formato exato que o agente deve retornar]
```

### Envelope de Resposta (sub‑agente → Orquestrador)
```
## RESPOSTA: [NOME_DO_AGENTE]
### estado
[sucesso | erro]

### resumo
[resumo legível de 2‑3 frases]

### dados
[lista numerada com pares chave‑valor, sem tabelas markdown]

### erros
[se estado for erro]
```

### Regras de tratamento de erros
1. Se o agente retornar `estado: erro`, o Orquestrador deve incluir o campo `erros` na resposta ao usuário e não prosseguir com passos dependentes.
2. Se houver falha em ferramenta externa (ex.: `firecrawl`), o agente deve registrar o erro em `erros` e abortar.
3. O Orquestrador nunca deve inventar dados; deve repassar exatamente o que o sub‑agente devolveu.

### Sequência de despacho do Coach (6 despachos)
1. Preparar perguntas de entrevista
2. Receber respostas do usuário
3. Avaliar respostas
4. Dar feedback
5. Sugerir melhorias
6. Concluir a sessão

(Os detalhes de cada passo serão definidos nas personas do Coach.)