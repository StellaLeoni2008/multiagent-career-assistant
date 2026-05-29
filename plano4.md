# Plano 4 – Agente Coach (Simulação de Entrevistas)

## Visão Geral

Desenvolver o agente **Coach** responsável por simular entrevistas de emprego com base na vaga desejada pelo usuário. O Coach gera perguntas personalizadas, misturando questões técnicas, comportamentais e situacionais, considerando o nível da vaga (estágio, júnior, pleno ou sênior) e a área de atuação. As perguntas são abertas e incentivam respostas detalhadas; o agente adapta as próximas perguntas com base nas respostas anteriores, simulando um entrevistador experiente.

## Ferramentas

1. **spawn_agent** – utilizado para despachar sub‑agentes internos que podem gerar perguntas ou analisar respostas, caso a lógica seja dividida em etapas.
2. **find_path** – para verificar a existência de arquivos de perfil do usuário em `data/user-profile.md`.
3. **read_file** – para ler o perfil do usuário e extrair informações da vaga desejada.
4. **edit_file** (opcional) – para armazenar o histórico da entrevista em `data/coach-history.md`.

## Estrutura de Skills

Criar o arquivo `skills/coach.md` contendo o procedimento de entrevista, incluindo:

- **Envelope de despacho** para sub‑agentes que geram perguntas ou avaliam respostas.
- **Formato de saída esperado** (lista numerada de perguntas e respostas, pares chave‑valor). Cada pergunta deve ter o campo `Pergunta` e, após a resposta do usuário, o campo `Resposta`.
- **Tratamento de erros**: registrar o erro em campo `erros` e abortar a operação se necessário.

## Passos de Implementação

### 1. Definir Prompt de Despacho
```
## DESPACHO: COACH
### referencia_persona
[Conteúdo completo de personas/coach.md]

### tarefa
Simular entrevista para a vaga especificada no perfil do usuário.

### perfil_usuario
[Conteúdo de data/user-profile.md]

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

### 2. Geração de Perguntas
1. **Classificação da vaga** – a partir do perfil, determinar a combinação de área + nível.
2. **Banco de perguntas** – manter um conjunto interno (hard‑coded) de perguntas técnicas, comportamentais e situacionais organizadas por área e nível.
3. **Seleção aleatória** – escolher 2‑3 perguntas técnicas, 2 comportamentais e 1‑2 situacionais, garantindo variedade.
4. **Personalização** – inserir o nome da vaga/cargo nas perguntas para torná‑las específicas.

### 3. Adaptação Dinâmica
Após cada resposta do usuário, analisar palavras‑chave (ex.: "não tenho experiência", "estou confortável", "já trabalhei com") e, com base nisso, escolher a próxima pergunta que aprofunde o ponto mencionado ou cubra lacunas detectadas.

### 4. Persistência do Histórico
- Salvar cada par Pergunta/Resposta em `data/coach-history.md` usando a mesma estrutura de lista numerada.
- Atualizar o arquivo a cada interação para que o Coach possa referenciar respostas anteriores.

### 5. Integração com o Orquestrador
- Quando o usuário escolher **C** no menu principal, o Orquestrador deve:
  1. Verificar se `data/user-profile.md` existe (usando `find_path`).
  2. Ler o perfil (`read_file`).
  3. `spawn_agent` com o envelope definido acima.
  4. Receber a **RESPOSTA** do Coach e exibir as perguntas ao usuário.
  5. Após a resposta do usuário, chamar novamente o Coach (novo despacho) para a próxima pergunta, até que um número definido de rodadas (ex.: 6) seja concluído.
  6. Ao final, apresentar um resumo das áreas que precisam de melhoria e sugestões de estudo (pode delegar ao Curator ou ao Scout).

### 6. Formato de Resposta do Coach
```
## RESPOSTA: COACH
### estado
sucesso

### resumo
Entrevista simulada iniciada para a vaga de Desenvolvedor Frontend Júnior. 6 perguntas serão feitas.

### dados
1. Pergunta: "Fale sobre um projeto recente onde você utilizou React. Quais foram os principais desafios e como você os superou?"
2. Pergunta: "Como você lida com prazos apertados e mudanças de requisitos durante o desenvolvimento?"
3. Pergunta: "Descreva uma situação em que você precisou colaborar com designers para melhorar a experiência do usuário."
4. Pergunta: "Qual foi a maior dificuldade técnica que você encontrou ao trabalhar com APIs REST e como a resolveu?"
5. Pergunta: "Conte uma experiência em que você recebeu feedback negativo e como reagiu a ele."
6. Pergunta: "Imagine que o cliente pede uma funcionalidade que você considera inviável dentro do prazo. Como você abordaria essa situação?"

### erros

```

## Próximos Passos
1. Criar a persona `personas/coach.md` descrevendo o comportamento, tom e estilo de comunicação do agente Coach.
2. Implementar a skill `skills/coach.md` seguindo o fluxo descrito.
3. Atualizar `skills/dispatch.md` para incluir a rota do agente Coach (opção **C**).
4. Testar a interação completa: escolha **C**, geração de perguntas, captura de respostas, adaptação dinâmica e resumo final.
5. Documentar quaisquer dependências adicionais (ex.: modelo de linguagem para análise de respostas) em `data/.env` ou similar.

---

Este plano complementa os planos anteriores, completando o ciclo de apoio ao usuário: **quiz → perfil → busca de vagas → cursos → simulação de entrevista**.
