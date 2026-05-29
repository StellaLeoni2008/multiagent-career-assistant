# Plano 2 – Agente Scout (Busca de Vagas)

## Visão Geral

Desenvolver o agente **Scout** responsável por buscar vagas de emprego em sites como **InfoJobs**, **Vagas.com** e **Indeed**. O agente será acionado pelo Orquestrador quando o usuário escolher a opção **A** no menu.

## Ferramentas

1. **firecrawl** – CLI já instalada no ambiente. Deve ser usada como primeira opção para raspar o conteúdo das páginas de vagas.
2. **Acesso Web nativo** – Caso o `firecrawl` falhe (erro de execução ou indisponibilidade), o agente deve recorrer ao mecanismo de navegação web interno do Zed (via `spawn_agent` ou `terminal` com `curl`/`wget`).

## Estrutura de Skills

Criar o arquivo `skills/scout.md` contendo o procedimento de busca, incluindo:

- Envelopes de despacho para sub‑agentes de extração de vagas.
- Formato de saída esperado (listas numeradas com pares chave‑valor: `Título`, `Empresa`, `Local`, `Link`, `Salário`).
- Tratamento de erros: registrar o erro em campo `erros` e abortar a operação se ambos os métodos falharem.

## Passos de Implementação

1. **Definir Prompt de Despacho**
   - Referência à persona `personas/scout.md` (a ser criada futuramente).
   - Tarefa: "Buscar vagas para o perfil do usuário em {sites}".
   - Dados de entrada: conteúdo de `data/user-profile.md`.
   - Saída esperada: lista de vagas no formato padrão.

2. **Implementar Execução com firecrawl**
   - Comando exemplo: `firecrawl crawl "https://www.infojobs.com.br/vagas?q={cargo}" --output json`.
   - Parsear o JSON e extrair os campos necessários.
   - Limitar a 20 vagas por site.

3. **Fallback – Navegação Web Nativa**
   - Utilizar `curl` ou `wget` para obter o HTML da página de busca.
   - Aplicar expressões regulares (ou `grep`) para capturar título, empresa e link.
   - Se ainda falhar, registrar erro.

4. **Agregação de Resultados**
   - Unir resultados de todos os sites, remover duplicados (mesmo `Link`).
   - Ordenar por relevância (por exemplo, correspondência de palavras‑chave do cargo).

5. **Persistência**
   - Salvar o resultado em `data/scout-results.md` usando a estrutura de lista numerada.
   - Atualizar campo `erros` se houver falhas.

6. **Integração com Orquestrador**
   - O Orquestrador, ao receber a escolha **A**, deve `spawn_agent` com o envelope definido acima.
   - Após a resposta, exibir ao usuário as vagas formatadas.

## Exemplo de Saída (`data/scout-results.md`)

```
1. Título: Desenvolvedor Frontend Júnior
   Empresa: TechCorp
   Local: São Paulo, SP
   Link: https://www.infojobs.com.br/vaga/12345
   Salário: R$3.500 - R$4.500

2. Título: Analista de Dados Pleno
   Empresa: DataSolutions
   Local: Remoto
   Link: https://www.indeed.com.br/viewjob?jk=abcde
   Salário: Não informado
```

## Próximos Passos

- Criar a persona `personas/scout.md`.
- Implementar o arquivo de skill `skills/scout.md` com o procedimento descrito.
- Atualizar o `dispatch.md` para incluir a rota do agente Scout.
- Testar a execução do `firecrawl` localmente e validar o fallback.
