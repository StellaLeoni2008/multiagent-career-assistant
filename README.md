<h1 align="center"> 💼 MultiAgent Career Assistant </h1>

<img width="1000" height="550" alt="a9cce696-34b0-43dd-9164-2dd2cde023c7" src="https://github.com/user-attachments/assets/f85449b1-9440-469f-a317-08e0507dc448" />

## 💻 Tecnologias utilizadas no projeto
<img loading="lazy" src="https://img.shields.io/badge/Zed-grey"/>      <img loading="lazy" src="https://img.shields.io/badge/OpenRouter-purple"/>      <img loading="lazy" src="https://img.shields.io/badge/Firecrawl-orange"/>      <img loading="lazy" src="https://img.shields.io/badge/Markdown-blue"/>

---

## 📌 Sobre
Este projeto foi desenvolvido durante a **Imersão IA da Alura**, com o objetivo de explorar a construção de sistemas multiagentes capazes de colaborar na resolução de problemas relacionados ao desenvolvimento de carreira.

A aplicação simula um assistente inteligente que auxilia usuários na busca por oportunidades profissionais, identificação de lacunas de habilidades, recomendação de cursos e preparação para entrevistas de emprego.

O sistema foi construído utilizando o editor **Zed**, integrado à **API OpenRouter**, com o modelo **gpt-oss-120b**, disponibilizado gratuitamente durante essa semana.

---

## 📖 Documentação Técnica
Esses são os meus prompts utilizados na construção dos agentes: [clique aqui](Prompts.Utilizados.na.Construcao.dos.Agentes.pdf)

---

## 🏗️ Arquitetura do sistema
O projeto utiliza uma arquitetura multiagente composta por agentes especializados que trabalham em conjunto para atender às solicitações do usuário.

### 🎯 Orquestrador
Responsável por:
* conduzir o quiz de perfil profissional
* consolidar informações do usuário
* gerar o perfil profissional
* controlar o fluxo principal da aplicação
* coordenar os demais agentes

### 🔎 Scout
Responsável por:
* buscar vagas compatíveis com o perfil do usuário
* identificar oportunidades profissionais relevantes
* consolidar resultados de múltiplas fontes
* apresentar vagas alinhadas aos objetivos do usuário

### 📚 Curator
Responsável por:
* identificar lacunas de habilidades
* recomendar cursos relevantes
* sugerir trilhas de aprendizado
* apoiar o desenvolvimento profissional contínuo

### 🎙️ Coach
Responsável por:
* simular entrevistas de emprego
* gerar perguntas personalizadas de acordo com a área e nível do usuário
* combinar perguntas técnicas, comportamentais e situacionais
* adaptar as próximas perguntas com base nas respostas anteriores
* fornecer feedback e identificar pontos de melhoria

---

## 🔄 Fluxo da aplicação
<p align="center">
<img width="387" height="450" alt="recoloca-a" src="https://github.com/user-attachments/assets/5fc0e9a5-3b03-40d0-b10e-6c7eeda38317" />
</p>

---

## 🧠 Conceitos praticados
Durante o desenvolvimento deste projeto, foram explorados conceitos importantes relacionados à Engenharia de IA, incluindo:
* Sistemas multiagentes
* Orquestração de agentes especializados
* Engenharia de prompts
* Estruturação de personas
* Protocolos de handoff entre agentes
* Persistência de estado
* Fluxos conversacionais com LLMs
* Arquiteturas orientadas a agentes
* Organização de contexto entre múltiplos agentes
* Construção de workflows inteligentes
* Desenvolvimento assistido por IA

---

## ▶️ Como executar o projeto
1. Clone este repositório:

```bash
git clone [https://github.com/StellaLeoni2008/multiagent-career-assistant]
```

2. Abra o projeto no editor **Zed**.

3. Configure sua chave da **API**.

4. Abra o chat do agente dentro do Zed.

5. Envie a instrução:
```text
Leia e siga as instruções definidas em @AGENTS.md
```

6. O Orquestrador iniciará o fluxo principal da aplicação e coordenará os agentes especializados.

---

## 📂 Estrutura do projeto
```
recoloca-ia/
├── .firecrawl/
│   └── cybersecurity_jobs.json
│
├── data/
│   ├── personality-quiz.md
│   ├── scout-results.md
│   └── user-profile.md
│
├── personas/
│   ├── coach.md
│   ├── curator.md
│   └── scout.md
│
├── skills/
│   ├── coach.md
│   ├── curator.md
│   ├── dispatch.md
│   └── scout.md
│
├── AGENTS.md
├── plano.md
├── plano2.md
├── plano3.md
└── plano4.md
```

---

<br> 

## Autora

| [<img loading="lazy" src="https://avatars.githubusercontent.com/u/237313711?v=4" width=115><br><sub>Stella Leoni</sub>](https://github.com/StellaLeoni2008) |
| :---------------------------------------------------------------------------------------------------------------------------------------------------------: |

---

<p align="right">
29/05/2026
</p>
