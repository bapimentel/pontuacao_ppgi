# Calculadora de Pontuação de Publicações — PPGI (IC/UFAL)

Página estática que calcula a pontuação de publicações (periódicos e conferências) no ano corrente e nas janelas móveis de 2, 3 e 5 anos, cruzando o currículo Lattes com as classificações Qualis. Todo o processamento ocorre localmente no navegador.

## Arquivos do repositório

- `index.html` — a ferramenta.
- `classificacoes_publicadas_computacao_2026.xlsx` — Qualis de periódicos, área de Computação (colunas ISSN, Título, Estrato).
- `classificacoes_publicadas_interdisciplinar_2026.xlsx` — Qualis de periódicos, área Interdisciplinar (mesmas colunas).
- `qualis-conferencias.xlsx` — Qualis de conferências da Computação (colunas Sigla, Título, Estrato).

A ferramenta lê as planilhas do próprio diretório por caminho relativo. Os dois arquivos de periódicos são combinados: a classificação de Computação tem prioridade e a Interdisciplinar completa os ISSN que não constam em Computação. A coluna Origem da tabela de revisão indica de qual área veio o estrato de cada periódico. O único arquivo enviado por upload é o Lattes, em XML.

Sobre as conferências: a fonte oficial da área é um PDF (`Qualis_CC-confs.pdf`) cujo texto não é extraível diretamente. A planilha `qualis-conferencias.xlsx` foi gerada a partir desse PDF (Sigla, Título e Estrato de 946 eventos) para que a ferramenta a leia como as demais. Você pode manter o PDF no repositório apenas como referência; a ferramenta usa a planilha.

## Publicar no GitHub Pages

1. Crie um repositório no GitHub e envie os três arquivos acima para a raiz (ou para uma pasta `docs/`).
2. No repositório, vá em `Settings` → `Pages`.
3. Em `Build and deployment`, escolha `Deploy from a branch`, selecione a branch `main` e a pasta `/ (root)` (ou `/docs`, se usou essa pasta). Salve.
4. Aguarde a publicação e acesse a URL indicada, no formato `https://SEU-USUARIO.github.io/NOME-DO-REPOSITORIO/`.

A leitura das planilhas depende de a página ser servida por HTTP (GitHub Pages ou um servidor local). Abrir o `index.html` por duplo clique (protocolo `file://`) não funciona, porque o navegador bloqueia a leitura de arquivos locais nesse modo. Para testar localmente, use um servidor simples, por exemplo `python3 -m http.server` no diretório dos arquivos, e acesse `http://localhost:8000`.

## Como usar

1. Na Plataforma Lattes, exporte o currículo em XML (menu `Exportar`). Envie o arquivo `.xml` (ou o `.zip` que o contém) no passo 1.
2. Ajuste, se necessário, o ano corrente e as opções no passo 2.
3. Confira ou edite a pontuação por estrato no passo 3. Todos os estratos ficam disponíveis para edição.
4. A ficha com os totais e o detalhamento aparece no passo 4. Na tabela de revisão, é possível corrigir o estrato de qualquer publicação; o total é recalculado na hora.

## Atualizar as classificações Qualis

Substitua os arquivos mantendo os mesmos nomes e as colunas descritas. Para periódicos, a ferramenta procura, em ordem de prioridade, `classificacoes_publicadas_computacao_2026.xlsx` (Computação) e `classificacoes_publicadas_interdisciplinar_2026.xlsx` (Interdisciplinar); aceita também `qualis-periodicos-computacao.xlsx`, `qualis-periodicos.xlsx`, `periodicos.xlsx` e `qualis-periodicos-interdisciplinar.xlsx`, `interdisciplinar.xlsx`. Para conferências, procura `qualis-conferencias.xlsx` ou `conferencias.xlsx`. Se você quiser incluir outras áreas de periódicos além dessas duas, me avise para acrescentar à lista.

## Observações sobre a pontuação

- Os valores por estrato seguem o regimento do PPGI (A1 = 1, A2 = 0,875, A3 = 0,75, A4 = 0,625, B1 = 0,5). Os estratos abaixo de B1 estão preenchidos por extensão linear (passo 0,125) e sinalizados no editor; ajuste-os conforme o regimento.
- As janelas móveis incluem o ano corrente: 2 anos = (ano−1 a ano), 3 anos = (ano−2 a ano), 5 anos = (ano−4 a ano).
- Periódicos são classificados por ISSN. Conferências são classificadas por nome exato, por sigla presente no nome do evento e por similaridade de termos; casos aproximados são sinalizados para revisão.
