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

## Pontuação e verificação (Resolução nº 1/2025)

Os valores por estrato seguem a tabela do regimento, idêntica para periódicos e conferências: A1 = 1, A2 = 0,875, A3 = 0,75, A4 = 0,625, B1 = 0,5. O regimento define a tabela apenas até B1; os estratos abaixo de B1 (B2 a C) valem 0 e não contam, pois os limiares exigem estrato igual ou superior a B1. Todos os campos são editáveis, caso queira simular outra configuração.

Divisão por coautoria: quando um trabalho é coautorado com outros docentes do PPGI, o regimento divide a pontuação do trabalho pelo número de coautores que são docentes do PPGI. A ferramenta lê os autores de cada publicação no XML do Lattes, identifica quais são docentes do PPGI (lista obtida em ic.ufal.br/pt-br/pos-graduacao/informatica/docentes, permanentes e colaboradores) e aplica a divisão. O número de coautores docentes de cada linha aparece na coluna "÷ PPGI" da tabela de revisão e pode ser corrigido manualmente; a pontuação efetiva é recalculada na hora.

A etapa "Verificação do regimento" avalia o critério Pesquisa nos últimos 36 meses (aproximados pela janela de três anos, ano−2 a ano, já que o Lattes informa apenas o ano):

- Credenciamento (Art. 3º): produção efetiva igual ou superior a 3,5 pontos em conferências ou periódicos com estrato igual ou superior a B1, e pelo menos dois periódicos A3 ou superior.
- Recredenciamento (Art. 4º): produção efetiva igual ou superior a 2,5 pontos nas mesmas condições, e pelo menos um periódico A4 ou superior.

Os critérios de Orientação e Docência (Art. 4º), a coautoria com discente e a exigência de produção majoritariamente na área de Computação e afins dependem de avaliação qualitativa e não são verificados automaticamente.

As janelas móveis da ficha incluem o ano corrente: 2 anos = (ano−1 a ano), 3 anos = (ano−2 a ano), 5 anos = (ano−4 a ano). Periódicos são classificados por ISSN; conferências por nome exato, por sigla presente no nome do evento e por similaridade de termos, com os casos aproximados sinalizados para revisão.
