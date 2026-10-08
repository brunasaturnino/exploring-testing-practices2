# Explorando Práticas de Teste

Neste exercício, vamos explorar práticas de teste em sistemas reais utilizando a ferramenta [TestMiner](https://andrehora.github.io/testminer).

O TestMiner permite visualizar e analisar testes de software em repositórios do GitHub, fornecendo dados sobre como os projetos organizam seus testes, como eles evoluem entre versões e quais bibliotecas de teste são utilizadas.
Explore a ferramenta antes de começar para se familiarizar com seu funcionamento.

Mais detalhes no GitHub da ferramenta: https://github.com/andrehora/testminer.

---

## Passo 1: Selecionar DOIS repositórios

Escolha dois repositórios reais que possuam testes de software.
Abaixo estão alguns links para ajudá-lo a encontrar projetos interessantes:

- Python: https://github.com/topics/python?l=python
- JavaScript: https://github.com/topics/javascript?l=javascript
- TypeScript: https://github.com/topics/typescript?l=typescript
- Java: https://github.com/topics/java?l=java

- Tópicos: [ai](https://andrehora.github.io/testminer/#topic:ai), [llm](https://andrehora.github.io/testminer/#topic:llm), [api](https://andrehora.github.io/testminer/#topic:api), [nodejs](https://andrehora.github.io/testminer/#topic:nodejs), [android](https://andrehora.github.io/testminer/#topic:android)

- Por organização: [Google](https://andrehora.github.io/testminer/#google), [Microsoft](https://andrehora.github.io/testminer/#microsoft), [Apple](https://andrehora.github.io/testminer/#apple), [Facebook](https://andrehora.github.io/testminer/#facebook), [Netflix](https://andrehora.github.io/testminer/#netflix), 
[GitHub](https://andrehora.github.io/testminer/#github), [Apache](https://andrehora.github.io/testminer/#apache), [HuggingFace](https://andrehora.github.io/testminer/#huggingface)

## Passo 2: Explorar os repositórios selecionados

Busque os repositórios escolhidos no [TestMiner](https://andrehora.github.io/testminer) e analise os dados de teste gerados pela ferramenta.

## Passo 3: Explicar as prática de teste

Para cada repositório, escolha uma prática ou dado de teste relevante e explique com suas próprias palavras.

---

## Instruções de entrega

1. Faça um `fork` deste repositório (saiba mais sobre forks [aqui](https://docs.github.com/pt/pull-requests/collaborating-with-pull-requests/working-with-forks/fork-a-repo)).
2. Responda às questões abaixo diretamente neste arquivo `README.md` do seu fork. Pode adicionar imagens para enriquecer sua explicação.
3. No Moodle, submeta apenas a URL do seu fork.

---

## Respostas

### Repositório 1

Repositório: https://github.com/fastapi/fastapi

URL TestMiner: https://andrehora.github.io/testminer/#fastapi/fastapi

Explicação:

O FastAPI é um framework Python para criar APIs. No TestMiner, a branch `main` aparece com cerca de 102,9 mil estrelas, 1.805 arquivos de código e 370 arquivos de teste, além de 95 test helpers, 13 benchmarks, 2 testes de CI e 1 smoke test. A maior parte desses arquivos fica na pasta `tests` (383 arquivos na visualização de localização).

A prática que mais chama atenção é o crescimento da suíte ao longo das releases. O gráfico de Test History compara três versões:

- na `0.1.11`, o projeto tinha só 4 arquivos de teste e 160 arquivos de código;
- na `0.95.2`, já eram 440 testes e 1.179 arquivos de código;
- na `0.143.0`, a suíte chegou a 620 testes, 146 helpers e 2.199 arquivos de código.

Ou seja, os testes não foram escritos de uma vez no começo. Eles foram acompanhando o framework conforme novas funcionalidades entravam. Na `main` atual o TestMiner conta 370 arquivos de teste, menos do que na tag `0.143.0`. Isso sugere que a linha de desenvolvimento recente reorganizou arquivos, e não que o projeto tenha abandonado os testes: ainda há uma pasta dedicada, helpers (como os vários `init`) e testes de segurança e de resposta (`security` com 33 arquivos e `response` com 22).

Para sustentar essa suíte, as dependências de teste listadas no SBOM são todas do ecossistema pytest: `pytest`, `pytest-cov` e `coverage` (cobertura), `pytest-xdist` (rodar testes em paralelo), `pytest-timeout` (evitar teste travado), `pytest-sugar` (saída mais legível) e `pytest-codspeed` (desempenho). Também aparece o `playwright`, usado para testes que passam por um navegador. Juntas, essas bibliotecas mostram um projeto que trata teste automatizado como parte contínua do desenvolvimento, e não como um passo isolado no fim.

### Repositório 2

Repositório: https://github.com/huggingface/transformers

URL TestMiner: https://andrehora.github.io/testminer/#huggingface/transformers

Explicação:

O Transformers, da Hugging Face, é a biblioteca Python usada para definir e treinar modelos de deep learning. No TestMiner ele tem cerca de 166,9 mil estrelas. Na `main` há 4.751 arquivos de código, 1.141 de teste, 599 test helpers, 77 fixtures, 26 benchmarks, 4 testes de CI e 1 smoke test. Quase tudo isso está concentrado na pasta `tests` (1.773 arquivos na localização). A única dependência de teste apontada no SBOM é o `pytest`.

A prática que escolhi é o uso de fixtures. Fixture, nesse contexto, é um arquivo com dado pronto que o teste reutiliza: um exemplo de entrada e, muitas vezes, a saída que o código deveria produzir. O TestMiner marca 77 fixtures, e os termos mais frequentes nos nomes são `expected` (42), `results` (33), `single` (14), `batch` (12) e `sample` (7). Isso combina com o tipo de verificação que essa biblioteca precisa fazer. Um teste de tokenização ou de visão não monta o exemplo inteiro dentro do código do teste; ele carrega uma amostra pequena (`sample`, `single` ou `batch`) e compara o resultado com um valor já conhecido (`expected` ou `results`). Se um modelo mudar a saída sem querer, o teste quebra.

Os próprios testes seguem as áreas do produto. Os nomes mais comuns são `modeling` (517), `processing` (287), `image` (131), `tokenization` (100) e `video` (48). A suíte também cresceu junto com o projeto: a release `0.1.2` tinha 3 testes (modeling, optimization e tokenization) e 19 arquivos de código; a `4.32.1` já tinha 645 testes e 32 fixtures; a `5.19.0` chegou a 1.139 testes e 77 fixtures. As fixtures dobraram entre a versão do meio e a mais recente, no mesmo período em que entraram mais testes de imagem, processamento e vídeo. O projeto não testa só “se o código roda”: ele guarda exemplos e resultados esperados para conferir se o comportamento dos modelos continua o mesmo.
