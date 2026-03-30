# Caderno Temático: LLMs e Engenharia de Prompt

Projeto para o desafio da DIO usando o NotebookLM como ferramenta de estudo.

---

## Contexto e Objetivos

### Tema
**Large Language Models e Engenharia de Prompt**

### Por que esse tema
Estudo Engenharia de Software na FIAP e trabalho com dados. LLMs aparecem em praticamente tudo que envolve processamento de linguagem no backend — e saber escrever um bom prompt faz diferença real no resultado. Escolhi esse tema porque é útil agora, não porque soa bem no portfólio.

### O que quero entender ao final
- Como LLMs funcionam de verdade — não a versão superficial
- A diferença prática entre zero-shot, few-shot e chain of thought
- Onde os modelos erram e por quê
- Um repertório de prompts que eu possa reusar em projetos reais

---

## Fontes

Selecionei 5 fontes abertas e fiz upload no NotebookLM:

| # | Fonte | Por que escolhi |
|---|-------|-----------------|
| 1 | [A Survey of Large Language Models — arXiv](https://arxiv.org/abs/2303.18223) | Paper acadêmico mais citado sobre arquitetura e capacidades dos principais LLMs |
| 2 | [Prompt Engineering Guide — promptingguide.ai](https://www.promptingguide.ai/) | Referência prática com exemplos reais de cada técnica |
| 3 | [OpenAI Cookbook — GitHub](https://github.com/openai/openai-cookbook) | Exemplos de uso real da API — mais concreto que documentação genérica |
| 4 | [Risks and Limitations of LLMs — MIT Technology Review](https://www.technologyreview.com/2023/04/19/1071436/large-language-models-gpt-risks/) | Perspectiva crítica sobre viés, alucinação e riscos éticos |
| 5 | [Chain-of-Thought Prompting — arXiv](https://arxiv.org/abs/2201.11903) | Paper original da técnica — vale ler a fonte antes de ler explicações de terceiros |

---

## Engenharia de Prompts — O que funcionou e o que não funcionou

### Prompt 1 — Começando errado

```
Me explique o que são LLMs.
```

**O que aconteceu:** Resposta genérica de três parágrafos que eu poderia ter encontrado na Wikipédia. O NotebookLM usou as fontes, mas não sabia o que eu queria de verdade.

**Ajuste:**
```
Com base nas fontes carregadas, explique o que são LLMs em dois blocos separados:
1. Como funcionam tecnicamente (arquitetura, parâmetros, tokens)
2. Como diferem de modelos tradicionais de machine learning
Não misture os dois blocos.
```

**O que mudou:** Muito melhor. Separar em blocos forçou o modelo a organizar antes de responder. O aprendizado aqui foi simples: prompt vago, resposta vaga.

---

### Prompt 2 — Few-shot para padronizar formato

Queria um glossário com formato consistente. Descrever o formato em texto não funcionou — o modelo interpretava diferente a cada geração.

```
Crie um glossário dos principais conceitos das fontes seguindo exatamente este formato:

**Token**: Unidade mínima de texto processada pelo modelo.
**Temperatura**: Controla a aleatoriedade das respostas — valores altos geram mais variação.

Continue com os 10 conceitos mais relevantes.
```

**O que aconteceu:** Seguiu o padrão sem desviar. Few-shot é mais eficiente do que descrever o formato — mostrar funciona melhor do que explicar.

---

### Prompt 3 — Chain of Thought em análise de riscos

```
Quais são os riscos éticos dos LLMs?
```

Resposta: lista de tópicos rasos. Sem causa, sem exemplo, sem mitigação.

**Ajuste com Chain of Thought:**
```
Pense passo a passo antes de responder. Para cada risco ético dos LLMs:
1. Explique a causa raiz
2. Dê um exemplo concreto
3. Sugira uma mitigação possível
```

**O que mudou:** A análise ficou três vezes mais útil. O "passo a passo" forçou o modelo a desenvolver o raciocínio antes de concluir — em vez de listar, ele explicou.

---

### Prompt 4 — Role Prompting para mudar o nível

```
Você é um professor universitário explicando LLMs para alunos do primeiro semestre. 
Resuma os conceitos principais das fontes sem jargão técnico excessivo. 
Use pelo menos uma analogia do cotidiano.
```

**Para que serviu:** Revisão inicial antes de aprofundar. O tom muda completamente — analogias aparecem, explicações ficam mais diretas. Útil quando o conteúdo está denso demais.

---

### Prompt 5 — Divisão de tarefa

Tentei pedir tudo de uma vez: resumo, glossário e questões de revisão num único prompt. O modelo misturou os três e o resultado foi inutilizável.

**Solução:** Três prompts separados, em sequência. Cada um com um objetivo único.

Parece óbvio em retrospecto. Não era na hora.

---

### Tabela de troubleshooting

| Problema | Causa | Solução |
|----------|-------|---------|
| Resposta genérica | Prompt sem contexto ou objetivo claro | Especificar formato, escopo e o que não incluir |
| Mistura de conceitos | Múltiplos objetivos no mesmo prompt | Um prompt por objetivo |
| Formato inconsistente | Descrição textual do padrão esperado | Few-shot — mostrar o exemplo |
| Análise rasa | Pergunta direta sem estrutura | Chain of Thought — pedir raciocínio passo a passo |
| Tom errado | Sem persona definida | Role Prompting — definir quem o modelo é |

---

## Miniguia de Estudo

### Como LLMs funcionam

LLMs geram texto token por token. Cada token é escolhido com base na probabilidade condicional dado o contexto anterior — não há "entendimento" no sentido humano, há predição estatística extremamente sofisticada.

O que muda em relação a modelos tradicionais é escala: bilhões de parâmetros treinados em trilhões de tokens. Mais parâmetros não garante mais inteligência, mas aumenta a capacidade de capturar padrões complexos na linguagem.

A janela de contexto é o limite do que o modelo processa de uma vez. Quando a conversa ultrapassa esse limite, o que ficou pra trás some — o modelo não lembra, simplesmente não acessa mais.

---

### Técnicas de Prompt

| Técnica | O que é | Quando usar |
|---------|---------|-------------|
| Zero-shot | Instrução direta, sem exemplos | Tarefas simples com resultado previsível |
| Few-shot | Exemplos de input → output antes da tarefa | Quando o formato da resposta precisa ser exato |
| Chain of Thought | "Pense passo a passo" | Análise, raciocínio lógico, problemas complexos |
| Role Prompting | Atribuir uma persona ao modelo | Controlar tom, nível técnico, perspectiva |
| Divisão de Tarefa | Um prompt por subtarefa | Qualquer prompt com mais de um objetivo |

Não existe técnica certa — existe a técnica certa para aquela tarefa. Testar e ajustar é parte do processo.

---

### Onde os modelos erram

**Alucinação:** O modelo gera informação falsa com tom confiante. Acontece especialmente em dados específicos — datas, nomes, números. Não é mentira intencional, é predição que deu errado.

**Viés:** Os dados de treinamento contêm preconceitos humanos. O modelo os reproduz, às vezes de forma sutil. Prompt tendencioso piora o problema — a pergunta já embute a resposta esperada.

**Janela de contexto:** Conversas longas perdem contexto. O modelo "esquece" o que foi dito no início quando a conversa ultrapassa o limite.

**Formato:** Sem instrução clara, o modelo escolhe o formato que estatisticamente aparece mais nas fontes de treinamento — que pode não ser o que você quer.

---

### Riscos Éticos

- **Reprodução de dados sensíveis:** Modelos podem ter memorizado informações privadas do treinamento e reproduzi-las
- **Desinformação em escala:** Gerar conteúdo falso ficou barato e rápido
- **Ausência de supervisão humana:** Sistemas sem revisão humana amplificam erros sem freio
- **Viés sistêmico:** Decisões automatizadas baseadas em modelos enviesados afetam pessoas reais

A mitigação passa por políticas de uso, comitês de ética durante o desenvolvimento e transparência sobre como os dados foram coletados e tratados. Não existe solução técnica que resolva isso sozinha.

---

### Glossário

| Termo | Definição |
|-------|-----------|
| **Token** | Unidade mínima de texto processada pelo modelo — pode ser uma palavra, sílaba ou caractere |
| **Janela de Contexto** | Limite de tokens que o modelo processa de uma vez — o que ultrapassa esse limite é ignorado |
| **Temperatura** | Parâmetro que controla aleatoriedade — alta gera mais variação, baixa gera mais previsibilidade |
| **Parâmetros** | Pesos aprendidos durante o treinamento — determinam o comportamento do modelo |
| **Alucinação** | Geração de informação incorreta apresentada com confiança |
| **Few-shot** | Técnica que fornece exemplos de input/output antes da tarefa principal |
| **Zero-shot** | Instrução direta sem nenhum exemplo |
| **Chain of Thought** | Técnica que pede ao modelo para mostrar o raciocínio antes de concluir |
| **Role Prompting** | Atribuição de uma persona ao modelo para direcionar tom e perspectiva |
| **Fine-tuning** | Retreinamento de um modelo base em dados específicos para especialização |
| **Prompt Engineering** | Prática de construir instruções deliberadas para extrair o melhor de um LLM |
| **IA Generativa** | Categoria de modelos que gera novo conteúdo a partir de padrões aprendidos |
| **Viés** | Preconceitos presentes nos dados de treinamento que o modelo reproduz nas respostas |

---

### Prompts reutilizáveis

**Para resumir documentos técnicos:**
```
Leia o documento e produza três itens separados:
1. Resumo de 3 parágrafos cobrindo os pontos principais
2. Lista de 5 conceitos-chave com definição de uma frase cada
3. 3 perguntas que esse documento responde

Não misture os três itens.
```

**Para gerar glossário padronizado:**
```
Crie um glossário dos termos mais relevantes seguindo este formato:
**[Termo]**: [Definição em uma frase direta]

Inclua os 10 termos mais importantes, do mais básico ao mais avançado.
```

**Para análise com raciocínio explícito:**
```
Pense passo a passo antes de responder:
1. Qual é o problema central?
2. Quais evidências ou argumentos sustentam a resposta?
3. Quais são as limitações ou pontos fracos?
4. Qual é sua conclusão?
```

**Para adaptar nível de complexidade:**
```
Você é um professor explicando este conteúdo para alguém sem background técnico.
Use linguagem direta, evite jargão sem explicação e inclua uma analogia do cotidiano.
```

**Para gerar questões de revisão:**
```
Crie 10 questões de múltipla escolha com 4 alternativas cada, cobrindo os conceitos principais.
Para cada questão: indique a alternativa correta e explique em uma frase por que as outras estão erradas.
```

---

## Autor

**Cauã Pereira da Silva**
Engenharia de Software — FIAP (2º semestre)

[![GitHub](https://img.shields.io/badge/GitHub-cauabackend-181717?style=flat&logo=github)](https://github.com/cauabackend)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-cauajava-0077B5?style=flat&logo=linkedin)](https://www.linkedin.com/in/cauajava/)