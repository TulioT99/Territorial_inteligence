# ProDignus — Guia mestre de expansão municipal dos dashboards

**Versão:** 3.0 — consolidada em 26/10/2026  
**Público:** Tulio, ChatGPT e Cursor  
**Base:** Goiânia, Porto Alegre, Curitiba e Florianópolis  
**Objetivo:** incluir uma cidade com rapidez, reutilizando o pipeline existente e preservando dados, metodologia e municípios publicados.

## 1. Como usar este guia

Leia este documento antes de iniciar uma cidade. Ele consolida e atualiza, para novas expansões municipais, o Roteiro Mestre v2 e a Decisão de Escopo e Governança v2. As duas cópias anexadas do Roteiro Mestre v2 são idênticas; não são versões diferentes.

A decisão de Porto Alegre de 30/08/2026 permanece como referência histórica e proteção dos contratos V2/C4O. Suas ordens temporárias de desativação, ativação e hotfix não devem ser reexecutadas numa nova cidade. Estado de batch, pointer e produção deve ser consultado, nunca inferido desse documento histórico.

Este guia não autoriza uploads, exclusões ou releases futuros. A preparação solicitada pelo usuário autoriza o trabalho local necessário dentro do escopo. Apresentar uma única entrega concreta para autorização das ações em produção. Autorizações já concedidas continuam válidas para seu escopo; não pedir a mesma autorização novamente.

**Princípio:** reproduzir antes de melhorar; corrigir somente divergências comprovadas; concluir a cidade antes de ampliar o projeto.

## 2. Avaliação dos documentos anteriores e alterações desta versão

| Tema | Problema ou lacuna anterior | Regra consolidada |
|---|---|---|
| Cadastro vazio | Admitia ausência de base → cobertura 0 como exceção operacional | Não usar esse atalho. Zero registros confirmados não prova cobertura populacional zero; campos de cobertura não validada ficam ausentes |
| Gates C0–C14 | Uma pergunta por gate e PARAR ao final multiplicavam interações | Preservar verificações, agrupadas em quatro fases; parar apenas por dependência real ou autorização de produção |
| Autenticação | AUTH BLOCKED interrompia toda a execução | Bloquear somente a parte dependente; continuar preparação independente e oferecer upload manual quando as sessões forem distintas |
| Mapas | Não explicitava o upload independente | XLSX não carrega PNGs. São quatro envios separados por cityCode e mapType |
| Legendas | Validação visual genérica | Comparar nomes, classes, ordem e cores com o PNG; reutilizar configuração municipal existente |
| Reversão | Backup sem delimitação operacional | Distinguir exclusão da importação, remoção dos mapas, recuperação de dados anteriores e rollback de release |
| V2/V3 | Risco de transformar histórico em estado atual | Provar precedência efetiva sem criar pointer ou alterar arquitetura |
| Referência territorial | Risco de exigir arquivo externo inexistente | Bairro oficial pode vir do IVT por join 1:1; logradouro ausente permanece vazio |
| Baseline | Risco de repetir cálculos e regressão integral em toda cidade | Reutilizar evidências válidas; aprofundar apenas se houver mudança de motor, fonte, contrato ou divergência |
| Homologação | Evidências locais e técnicas podiam parecer smoke visual | Declarar separadamente banco/API, imagem publicada, bundle e tela autenticada |

## 3. Escopo e limites

Permitido na preparação: inventariar, consultar produção somente leitura, reutilizar fontes, calcular IVT se ainda não existir artefato qualificado, gerar XLSX e mapas, executar parser isolado, corrigir minimamente parametrização/apresentação necessárias e preparar commit/PR quando houver código alterado.

Não incluir: revisão de pesos, imputações ou normalização; substituição de IVT_V2 por IVT_C4O_V1; nova facade, batch, migration, pointer, importador, plotter ou arquitetura cloud; refatoração não necessária; dados artificiais; alteração de outros municípios.

Os ramos municipais explícitos de **apresentação** das legendas são aceitos pelo fluxo existente. Não introduzir ramos ocultos que mudem o cálculo do IVT para uma cidade.

Divergência deve ser classificada em fonte, cálculo, processamento, artefato, importação, banco/API, seleção da fonte, agregação/UI, ambiente, autenticação ou deploy. Corrigir a primeira camada comprovadamente incorreta. Não reimportar para investigar uma divergência visual.

## 4. Fluxo ágil em quatro fases

| Fase | Trabalho agrupado | Saída e parada |
|---|---|---|
| A — Diagnóstico único | Ambiente, fontes/universo, IVT existente, base cidadã, produção e precedência | Inventário curto; continuar salvo dependência real |
| B — Pacote completo | XLSX, quatro mapas, legendas, rehearsal e checks pertinentes | Pacote congelado e uma solicitação de autorização |
| C — Execução autorizada | Publicar Admin se necessário; usuário importa XLSX e mapas manualmente, salvo outra preferência explícita | IDs, estados e artefatos publicados; não duplicar envios |
| D — Reconciliação e encerramento | Banco/API, hashes dos mapas, fonte efetiva, UI disponível e preservação das cidades | GO/NO-GO e limitações objetivas |

Não gerar relatório por gate. Manter um registro municipal curto, atualizado ao final, com arquivos/hashes, contagens, importação, release se houver, verificações e pendências. Não criar infraestrutura de manifesto ou auditoria nova para isso.

Reutilizar resultados qualificados. Não baixar novamente fontes, recalcular IVT, refazer mapas ou repetir testes aprovados sem mudança ou divergência que justifique. Paradas por autenticação não invalidam as etapas já concluídas.

## 5. Fase A — ambiente e fontes

### 5.1 Localizar a árvore correta uma vez

Referências operacionais observadas em outubro/2026; confirmar disponibilidade, não presumir acesso pelo ChatGPT:

- Árvore operacional: `C:\Users\Tulio Personal\Documents\004 - Algoritimos\censo_setor_pipeline`.
- Origem do build/insumos: `C:\Users\Tulio Personal\Documents\005 - Projeto Hercules\1 - Agencies and Consultings\00 - IDEATION (Tulio)\censo_setor_pipeline - Copia`.
- Python validado: `C:\Programs_Hercules\envs\maptest2\python.exe`.
- Build: `scripts/build_dim_setor.py` na Copia; argumento opcional `CIDADE UF`; sem argumentos preserva Goiânia no comportamento observado.
- Plot: `scripts/plot_mapa_ivt_v2.py` na árvore principal.
- Projetor: `scripts/card3_upload_v2_from_plot.py`.
- Rehearsal ExcelJS: `scripts/card3_upload_v2_exceljs_roundtrip.js`.

A Copia tinha plot fixado em Goiânia e sem records_v1; a árvore principal tinha parametrização e records_v1. Não substituir o script atual por backup V6 inteiro. Copiar apenas insumos necessários e conferir hashes. Respeitar AGENTS.md e o estado local do Git; preservar campanhas e outras alterações não relacionadas.

Ambiente validado: Python 3.10.20; geopandas 1.1.3; pyogrio 0.11.0; shapely 2.1.2; fiona 1.10.1; pandas 2.3.3; contextily 1.7.0. Isto é referência de compatibilidade, não ordem para reinstalar. Investigar bibliotecas nativas somente se falharem.

### 5.2 Fontes e universo

Reutilizar agregados nacionais Censo 2022, dicionário e malha oficial já qualificados. Registrar edição/hash. A variável populacional qualificada foi `v0001`, “Total de pessoas”, no dicionário `dicionario_de_dados_agregados_por_setores_censitarios_20260520.xlsx`. Confirmar o mesmo contrato quando mudar a fonte.

Malhas PR/SC usadas: shapefile oficial Censo 2022, EPSG:4674, com .shp/.shx/.dbf/.prj/.cpg. Um GPKG estadual não é automaticamente o mesmo produto; não trocar por conveniência.

- Extrair município pelo código IBGE; `CD_SETOR` como texto único de 15 dígitos, prefixo municipal de sete dígitos.
- Reconciliar conjuntos do básico e da malha, população, ausentes/suprimidos, geometrias e setores pop=0.
- Distinguir universo oficial/build de universo efetivo de mapas/upload. Nas cidades desta execução, o build inclui pop=0; mapas e XLSX usam pop>0.
- Não aplicar uma contagem fixa de Goiânia a outra cidade; nem confundir sua lista oficial com as 2.304 linhas históricas publicadas.

### 5.3 IVT

Reutilizar CSV congelado com hash validado, se existente. Calcular apenas quando necessário com o mesmo motor. Pesos observados: renda 0,25; educação 0,20; saneamento 0,20; habitação 0,20; adensamento 0,15; clipping final [0,1].

Na primeira geração, conferir componentes, recomposição em amostra, min/média/mediana/p90/max, zeros/nulos e imputações. Comparar processamento com o baseline, sem exigir distribuições municipais semelhantes. Evidências do mesmo motor já aprovadas podem ser reaproveitadas; aprofundar ao detectar mudança ou anomalia.

Imputações legadas e componentes acima de 1 antes do clipping são limitações conhecidas, não autorização para revisão metodológica. Nos pop=0, o tratamento observado imputa medianas municipais de renda/educação e zera demais componentes. Não corrigir durante a expansão municipal.

Diferenças de representação numérica devem ser avaliadas contra a serialização existente. Florianópolis apresentou diferenças de até 5,55e-17 pela regra `.16f`; isso não justificou regenerar o candidato ou alterar o motor. Não usar essa tolerância como licença genérica para divergências maiores.

## 6. Base cidadã: três situações distintas

| Situação comprovada | Uso permitido | Apresentação e campos |
|---|---|---|
| Base presente, piloto ou não representativa | Contar registros deduplicados e calcular razão diagnóstica registros/população IBGE | Identificar a base; não chamar a razão de cobertura populacional; cobertura e derivados não validados ficam ausentes |
| Base confirmada vazia em consultas | Registrar zero registros; não fabricar exportação individual | Mapa cadastral neutro, “Base cidadã confirmada vazia” / “Sem prioridade inferida”; isto não é cobertura populacional zero |
| Estado desconhecido ou autenticação bloqueada | Continuar trabalho independente | Não assumir zero, vazio ou cobertura; concluir diagnóstico antes de fechar artefatos dependentes |

Uma metodologia de cobertura populacional validada, se existir em outro projeto, exige contrato identificado. Não substituir a metodologia histórica de Goiânia pelas regras da base piloto sem decisão específica.

Consultar município/código e setor na base real, no tenant relevante; verificar outros tenants apenas quando necessário para esclarecer a localização. Ausência de CSV não prova banco vazio. Registrar data da consulta, filtros, consentimento/estado lógico pertinentes e limites da seleção.

Quando houver pessoas, exportar apenas identificador técnico e `CD_SETOR` quando suficientes. Não exportar nomes, telefones ou endereços desnecessários. Usar `records_v1`; `legacy` interpreta linhas como domicílios e multiplica por V00039, portanto não serve para este arquivo de pessoas.

Deduplicar pelo mecanismo existente com identificador e setor. Usar somente CD_SETOR colapsa pessoas diferentes. Identificadores com setores conflitantes exigem quarentena conforme a regra existente.

Reconciliar selecionados = exportados + sem setor + demais exclusões pertinentes. Distinguir setor inválido, fora do município e setor válido pop=0. Quem está em pop=0 não é “sem setor”; registrar sua exclusão do universo efetivo.

Exemplo Curitiba: 610 selecionados; 609 com setor válido; 1 sem setor; dos 609, um em pop=0; 608 registros efetivos em 515 setores. Não confundir 608/609/610 nem apresentar o piloto como cadastro municipal completo.

Limite observado no importador de cidadãos: em CPF existente, o caminho consultado atualizava vínculo institucional, sem persistir `ibge_census_sector`. A expansão territorial não autoriza corrigir esse importador. Associação por arquivo/janela/cidade não prova rastreabilidade individual perfeita.

## 7. Estado de produção e precedência — antes de gerar o pacote final

Consultar somente leitura V2 em `dashboard_ibge`, mapas em `dashboard_ibge_maps`, lotes em `dashboard_ibge_import_batch` e pointer em `city_dashboard_pointer`. Identificar tenant/escopo aplicável a cada fluxo, sem presumir que dados territoriais têm o mesmo isolamento de cidadãos.

Provar como o loader publicado escolhe V2/V3. Em Curitiba e Florianópolis, sem lote/pointer, o caminho efetivo usa V2. Não criar pointer para “ativar” V2. Se houver precedência concorrente, resolver a decisão antes da carga.

Rastrear UI → hook/store → loader → endpoint → contrato → campo → agregação apenas nas partes necessárias ou divergentes. Reutilizar o rastreamento estabelecido do mesmo código publicado.

Preservar `RawSector` V2 completo. O histórico de Porto Alegre mostrou que um envelope reduzido com HTTP 200 podia impedir fallback e perder campos analíticos. Não passar V2 por conversão que elimine `actionPriority`, `registrationGapIndex`, `vulnerabilityType`, `clusters`, `card3Summary`, `recommendedPolicy` ou `coverageClassification`. Não usar 401/403/500 como ausência de dataset. Confirmar regras atuais de fallback em vez de reimplementar arquitetura.

## 8. Fase B — XLSX candidato

Gerar uma vez a partir dos insumos qualificados, em pasta municipal nova. Congelar por caminho absoluto, tamanho, SHA-256, data, linhas, população, versão e metodologia. Correção posterior implica novo hash e substituição explícita do candidato aprovado.

- Aba inicial `Template`, 20 colunas, setores como texto, sem fórmulas.
- Bairro oficial pode vir do IVT por join 1:1 de CD_SETOR; validar nomes, unicidade e pertencimento. Não exigir CSV separado se a fonte já existe.
- Logradouro fica vazio quando ausente; não inventar referência.
- IVT e prioridade territorial preservados.
- Sem cobertura populacional validada: Classificação de Cobertura, Índice de Lacuna, Matriz e Ação permanecem vazios; não preencher zero.
- Resumo identifica “Registros da base piloto” e, quando aplicável, “Taxa registros/população IBGE”; nenhuma estimativa de pessoas cadastradas.
- ID da Importação da planilha pode ficar vazio: o vínculo persistido vem do fluxo da fila.

Ordem de referência do contrato; conferir rótulos exatos no parser atual, não reconstruir o importador a partir desta lista:

1. Código do Setor
2. Código do Setor (Dashboard)
3. Bairro
4. Logradouro
5. IVT v2
6. Classificação de Cobertura
7. Índice de Lacuna de Cadastro
8. Prioridade de Ação
9. Matriz IVT x Cobertura
10. Ação da Matriz IVT x Cobertura
11. População Total
12. Principal Ofensor
13. Tipo de Vulnerabilidade
14. Política Recomendada
15. Resumo (Card3)
16. Tipo de Vulnerabilidade (Combinado)
17. Chave Combinada de Políticas
18. Macroclusters
19. Clusters
20. ID da Importação

Executar parser real isolado, sem banco. Reutilizar roundtrip ExcelJS e testes existentes; testar apenas partes ainda não cobertas. Conferir contagens, nulos, população, bairros, IVT, clusters, tipos e substituição municipal. Um teste com mocks não prova sozinho resolução contra o catálogo publicado.

Comparar clusters com o catálogo real quando acessível; seed é evidência de compatibilidade esperada, não consulta atual de produção. Documentar relação vazia e sua aceitação pelo importador; não criar clusters para preencher. As restrições municipais históricas CC04/CC06 e o bloqueio por CC12 de Goiânia não se transportam automaticamente para outras cidades. Não afirmar regressão integral se parte da suíte histórica foi ignorada.

## 9. Quatro mapas e legendas — produto separado do XLSX

**O XLSX não envia mapas.** Os PNGs têm pipeline de publicação independente, inclusive reversão independente.

| Arquivo padrão municipal | mapType | Conteúdo |
|---|---|---|
| mapa_ivt_v2_<cidade>_<uf>.png | territory-vulnerability | Vulnerabilidade no território |
| mapa_prioridade_<cidade>_<uf>.png | social-intervention-priority | Prioridade de intervenção social |
| mapa_tipologia_territorial_<cidade>_<uf>.png | vulnerability-types | Tipos de vulnerabilidade |
| mapa_cobertura_cadastro_<cidade>_<uf>.png | social-registration-gaps | Cadastro conforme a evidência, sem inferência indevida de cobertura |

O nome do quarto arquivo e seu mapType podem continuar históricos. Título, texto e legenda devem refletir o conteúdo real.

### 9.1 Fundo CARTO e ambiente

O gerador atual usa `CARTO_BASEMAP_API_KEY`. Verificar sua presença sem imprimir o valor antes de gerar. A chave configurada em um PowerShell não é automaticamente herdada por outro processo ou terminal. Não gravar segredo no repo, prompt, relatório, URL exibida ou exceção.

Se necessário, usuário copia a chave e executa na sessão que iniciará o gerador:

```powershell
$env:CARTO_BASEMAP_API_KEY = (Get-Clipboard -Raw).Trim()
```

Isso configura a sessão atual. Se o Cursor não a herdar, fornecer o comando existente de geração para o usuário executar nesse terminal. Não repetir buscas de token ou configuração de máquina.

HTTP 200 e PNG não bastam: o CARTO sem autenticação retornou uma imagem “API KEY REQUIRED”. Validar visualmente que o fundo é cartográfico. Reutilizar o provedor/configuração atual; não reabrir a investigação do backup V6.

### 9.2 Qualidade e semântica

Conferir contorno, projeção apropriada (EPSG:3857 no plot observado), enquadramento, classes, cores, contagens, título, resumo, seta Norte em todos e atribuição inteira legível “(C) OpenStreetMap contributors (C) CARTO”.

O quadro de resumo encobriu a atribuição no gerador usado. Uma faixa externa legível resolveu sem recalcular indicadores. Preferir produzir a apresentação correta na primeira geração; se corrigir raster existente, preservar geografia/classes e guardar original e hash novo. Não transformar isso em refatoração do plotter.

A legenda do Admin e a do PNG não podem se contradizer. Verificar nomes, ordem, classes discretas/contínuas e cores (inclusive transparência que afete a percepção). Não inserir classe ausente como se estivesse desenhada. Não é obrigatório duplicar caixas de legenda dentro do PNG se a legenda da tela é suficiente e corresponde ao conteúdo; imagem isolada pode precisar de contexto adicional conforme seu uso.

Referência de Curitiba/Florianópolis, a confirmar contra o gerador da nova cidade:

| Camada | Classes/cores observadas |
|---|---|
| Vulnerabilidade | Cinco classes discretas: #006837, #86CB66, #FFFFBF, #F98E52, #A50026; alfa 0,90 no plot observado |
| Prioridade | Monitorar #E5E7EB; Média #FACC15; Alta #FB923C; Máxima #DC2626 |
| Tipologia | Econômica #f6d55c; escolaridade #3caea3; infraestrutura #ed553b; adensamento #20639b; combinada #8e44ad |
| Base piloto de Curitiba | Máxima #d73027; Média #fee08b; Não prioritário #f2f2f2; sem declarar cobertura populacional |
| Base vazia de Florianópolis | Único segmento #F2F2F2, “Sem prioridade inferida” |

Os mapas 1, 2 e 4 não usam necessariamente a mesma definição de “crítico”/“prioritário”; não igualar suas contagens.

Para cadastro piloto, não deduzir “cobertura adequada”, “busca ativa” ou população cadastrada a partir da razão diagnóstica. Curitiba manteve cores da prioridade operacional já calculada, com aviso explícito. Isso não autoriza reproduzir prioridades não validadas em outra base: confirmar o contrato do artefato antes de desenhar.

Para base confirmada vazia, usar quarto mapa neutro, sem vermelho/laranja/amarelo de prioridade cadastral: “Base cidadã confirmada vazia”, “Sem prioridade inferida” e “Isto não é cobertura populacional zero”.

### 9.3 Admin: mudar apenas se necessário

Reutilizar `TerritorialMapLegend` e `resolveTerritorialMapLegend`/configuração por cityCode existentes. Ajustar títulos, legendas e painel somente onde o contrato da cidade exigir. Preservar os demais municípios; não criar framework novo.

Se a configuração existente já representa a cidade corretamente, não criar PR ou release por rotina. Caso necessário, preparar diff mínimo, preview das quatro abas e testes direcionados mais checks obrigatórios do repo. Testes já aprovados não precisam ser repetidos sem mudança relevante. Não normalizar CRLF/LF em centenas de arquivos alheios ao diff por falha local de Prettier; avaliar arquivos alterados e CI real.

## 10. Autenticação e operação manual sem ciclos de tentativa

Distinguir sessão AWS/SSO, sessão do Admin do usuário e sessão do navegador controlado pelo agente. Login em uma não autentica outra.

Se o agente não alcança o navegador autenticado do usuário, adotar upload manual cedo, conforme preferência de Tulio. Não insistir em sucessivos pedidos para entrar numa aba inacessível. Não pedir senha/token na conversa, vasculhar tokens alternativos, resetar credenciais ou fabricar sessão.

Marcar `AUTH BLOCKED` somente nas verificações dependentes. Continuar geração e preparação independentes. Sem acesso ao banco local/produção, ChatGPT deve informar o limite e encaminhar ao Cursor o comando necessário; não afirmar execução ou GO que não verificou.

O usuário fazer upload manual não dispensa reconciliação. O executor não deve enviar o mesmo arquivo depois do usuário.

## 11. Fase C — autorização e execução

### 11.1 Solicitação única e concreta

Apresentar XLSX e quatro PNGs com caminhos/hashes; município/cityCode; contagens/população; semântica cadastral; dados preexistentes; fonte efetiva; rehearsal e condições; PR/SHA/checks/versão se necessário; responsáveis por cada ação; reversão e sequência.

Não pedir autorização final com mapas ainda inexistentes ou correspondência visual ainda não verificada, salvo decisão explícita de entregar por partes. Condições não impeditivas devem ser descritas sem abrir gates adicionais.

### 11.2 Ordem preferida após autorização

1. Se necessário, confirmar head/checks/tag livre, mergear PR e publicar Admin.
2. Confirmar deploy, imagem/digest e bundle efetivamente servidos.
3. Usuário importa um único XLSX municipal.
4. Aguardar resultado final antes dos mapas.
5. Usuário envia quatro PNGs, um por aba, conferindo município a cada envio.
6. Executar reconciliação única.

A autorização é específica ao SHA/escopo e arquivos congelados. Se head divergir, tag ocupar ou artefato mudar, informar a divergência antes de executar. Não reutilizar números de release antigos automaticamente.

### 11.3 XLSX: fluxo existente e risco principal

Admin: `/imports` → Importar Base → Dashboard → UF → Município → arquivo aprovado → Importar Arquivo uma vez.

Endpoint observado: `POST /file-imports/dashboard-ibge`, autenticado como admin. **O município vem do pedido; o XLSX não o define.** Conferir `cityCode` efetivo antes do POST. O `clearByCity` ocorre antes da fila: selecionar outra cidade pode apagar suas linhas mesmo com arquivo correto.

Registrar ID retornado/identificado da fila/importação, nome, horário, tipo, status, total_rows e itens de sucesso/erro. O `file_import_id` persistido vem da fila, não da coluna vazia da planilha.

Em timeout, resposta ambígua, falha parcial ou demora, consultar estado antes de qualquer nova tentativa. Não repetir POST, clicar novamente ou usar lixeira para “destravar”. Divergência gera diagnóstico, não reimportação automática.

### 11.4 PNGs: quatro envios independentes

Admin: `/dashboard` → selecionar UF/município → seção Prioridades Territoriais de Ação Social → aba correta → Carregar imagem. O botão requer perfil admin. Conferir cabeçalho municipal a cada arquivo.

Endpoint: `POST /dashboard-ibge/maps`, campos `file`, `cityCode`, `mapType`. PNG/JPEG/WEBP eram aceitos no contrato observado. A chave lógica é (cityCode, mapType); novo envio substitui imagem do mesmo par. Não há vínculo ao ID da importação na tabela observada.

Não enviar a pasta de previews, mapas antigos nem CSVs intermediários. Confirmar hashes dos arquivos finais. A existência do XLSX importado não prova que imagens foram publicadas.

## 12. Reversão: limites que precisam estar claros

| Ação | Procedimento observado / limite |
|---|---|
| Desfazer importação nova em cidade antes vazia | `DELETE /file-imports/:id` remove linhas dashboard_ibge vinculadas; FK remove relações dashboard_ibge_clusters. Volta a zero linhas se o cityCode estiver correto e não houver alterações posteriores conflitantes |
| Recuperar cidade que já tinha dados substituídos | O delete da importação nova não restaura dados antigos. Exige fotografia/backup recuperável e plano definido antes da carga |
| POST com cityCode errado | Apagar a importação não restaura a cidade limpa pelo clearByCity anterior à fila |
| Desfazer mapas | Independente do delete da importação. Remover/restaurar especificamente os pares afetados pelo mecanismo suportado; confirmar procedimento antes de agir |
| Rollback do Admin | Seguir procedimento existente para versão anterior compatível; não altera automaticamente dados nem mapas |
| Backup de cluster | Salvaguarda adicional; retenção de sete dias foi observada em hercules-prd. Verificar validade atual. Não equivale a reversão municipal pronta |

Nenhuma exclusão/reversão automática por divergência. Informar impacto e obter instrução específica, salvo autorização de reversão já expressa.

## 13. Fase D — reconciliação final e critério de conclusão

Uma passagem somente leitura, com esperado versus encontrado:

1. Importação SUCCESS, ID, arquivo, horário, total_rows e itens aceitos.
2. CityCode, setores únicos, prefixo/texto e linhas vinculadas à importação; população e igualdade dos campos essenciais com candidato, considerando serialização conhecida.
3. IVT, bairros, logradouro, prioridade, tipologia e resolução de clusters; relações vazias explicitadas.
4. Cobertura, lacuna, matriz e ação ausentes quando não validadas, sem virar zero. Zero válido continua zero. `registrationGapIndex=1` corresponde a cobertura zero válida no cálculo vigente; não confundir índice zero com cobertura zero.
5. Lote/pointer/fonte efetiva e endpoint que alimenta o dashboard; preservar campos analíticos.
6. Quatro mapas: pares corretos, conteúdo servido e hash dos bytes iguais aos PNGs aprovados. Datas isoladas não substituem hash quando há suspeita.
7. Admin publicado: versão, digest, deploy e bundle; correspondência das legendas.
8. Tela autenticada disponível: seleção da cidade, cards, cobertura indisponível quando aplicável, mapas/legendas, detalhe/drilldown. Não criar dados artificiais para testar nulo/zero; usar testes isolados e evidência técnica quando não houver linha real apropriada.
9. Preservação das cidades existentes: contagens/populações e mapas de controle, mais regressão curta do caminho compartilhado se código mudou. Não regenerar seus artefatos.

**Níveis de evidência:** banco/API prova persistência; hash prova imagem publicada; bundle prova código servido; preview local prova apresentação local; somente inspeção da tela autenticada prova smoke visual integrado. Não chamar inspeção do bundle de smoke visual aprovado.

Encerrar com GO quando as evidências necessárias estiverem reconciliadas. Se faltar visual autenticado, registrar “carga técnica reconciliada; smoke visual pendente”, em vez de afirmar homologação visual completa. NO-GO deve indicar divergência impeditiva, não uma melhoria opcional. Não confundir bloqueio de acesso com defeito comprovado dos dados.

Registro final curto: município, data, fontes/artefatos/hashes, universos, base cidadã, importação, mapas, fonte consumida, release se houver, verificações, limites e decisão. Não exigir múltiplos relatórios para a mesma conclusão.

## 14. Controles históricos — referências, não valores atuais garantidos

| Município | Código | Oficial / pop>0 / pop=0 | Upload V2 observado | População do upload |
|---|---|---|---|---|
| Curitiba/PR | 4106902 | 3190 / 3178 / 12 | 3178 | 1773718 |
| Florianópolis/SC | 4205407 | 1004 / 964 / 40 | 964 | 537211 |
| Goiânia/GO | 5208707 | 2310 oficiais recontados; não equiparar ao upload legado | 2304 | 1437366 |
| Porto Alegre/RS | 4314902 | Não requalificado neste ciclo | 2713 linhas observadas; fonte V2/V3 deve ser consultada | Não fixada neste guia |

### 14.1 Curitiba — reconciliada no ciclo

- Importação: `3d16dd2c-e7c7-4236-9eac-5ff3283c67a7`, SUCCESS, 26/10/2026.
- 75 bairros; lacuna nula em 3178 linhas; 310 IVT zero; três setores sem cluster aceitos.
- XLSX: `f2d957972069061ae1032cba3cd3104ef8669efd504c71025fefe99c5b23e988`.
- Fonte efetiva V2, sem pointer/lote V3 na consulta do ciclo.
- Quatro PNGs corrigidos publicados e hashes reconciliados; Goiânia/Porto Alegre preservadas conforme relato do executor.

### 14.2 Florianópolis — artefatos e publicação; reconciliação pós-carga não recebida

- Base cidadã confirmada vazia nas consultas do ciclo; isto é uma fotografia temporal.
- 88 bairros; 167 IVT zero; prioridades 676 Monitorar, 41 Média, 146 Alta, 101 Máxima.
- XLSX: `31382C2E582188D503A0E0C94B0F9BF4C5D0365CC50491EBB88CF93FA6FC4D85`.
- Usuário confirmou upload do XLSX e dos quatro PNGs. Até esta consolidação, não foi recebido o resultado da reconciliação final de produção. Não inventar ID, estado SUCCESS ou homologação final.

### 14.3 Releases e proteção das cidades

- Admin `0.0.216`: preservação de lacuna ausente como indisponível, sem zerar; agregação parcial identificada.
- `0.0.217`, PR #29: legendas de Curitiba; merge `bfeec10bdcf126e8fc5ec6b7a274a458c70f4757`.
- `0.0.218`, PR #30: legendas de Florianópolis; merge `19c0a0ecc15ea5e8fb17006bb1831267da03305e`; digest `sha256:e6df75410e5627286e9e8ef0a44da87aba682b4359d3efd915f300cd5883ac11`.
- Goiânia: quatro mapas de 03/08/2026. Porto Alegre: três JPEG de 30/08/2026, sem social-registration-gaps. A regressão preserva o estado real; não exige inventar um quarto mapa de Porto Alegre.
- Valores antigos de Goiânia (IVT médio ~0,2191; cobertura ~78,09%; outros KPIs de agosto) são referências condicionadas à fonte/contrato, não metas a forçar em nova cidade.

Os dados de produção acima vêm dos resultados do executor e confirmações do usuário nesta conversa, não de nova consulta realizada para redigir este guia.

## 15. Prompt reutilizável — próxima cidade

```text
PREPARAR DASHBOARD MUNICIPAL — ENTREGA ÚNICA

Leia ProDignus_Guia_Mestre_Expansao_Municipal_Dashboards_v3.md.
Município: <CIDADE/UF>. Código IBGE: <CITYCODE>.

Faça diagnóstico único: fontes/universo, IVT existente, base cidadã real,
dados/mapas V2 e lote/pointer V3, precedência efetiva do dashboard e acesso.
Reutilize artefatos qualificados e o ambiente existente.

Prepare XLSX e quatro PNGs numa passagem, com bairros oficiais, campos
não validados ausentes, sem inventar população cadastrada ou cobertura.
Use records_v1 para registros de pessoas. Base desconhecida não é vazia;
base confirmada vazia exige mapa cadastral neutro.

Valide o candidato no parser real isolado, nulos, clusters e substituição.
Confira PNGs, atribuição, Norte e correspondência das legendas no Admin.
Se necessário, ajuste somente a configuração municipal existente e prepare
PR com checks obrigatórios. Preserve as cidades já publicadas.

Não repetir gates/testes concluídos sem nova evidência. Não recalcular IVT
qualificado, refatorar, alterar metodologia ou criar arquitetura.
Se houver bloqueio de acesso, pare apenas a parte dependente e continue
as demais. Informe a intervenção mínima; sem busca de tokens alternativos.

Entregue uma única solicitação: caminhos/hashes de XLSX e PNGs, contagens,
população, semântica cadastral, fonte efetiva, condições, reversão e PR/SHA/
checks/versão se necessário. Eu farei os uploads manualmente, salvo instrução
expressa diferente. Planeje publicar Admin primeiro, se houver alteração.

Não importar, enviar imagens, excluir dados, mergear ou publicar antes da
autorização final. Após autorizado, executar só a parte atribuída ao agente;
aguardar confirmação dos uploads manuais e reconciliar somente leitura.
```

## 16. Checklist mínimo de encerramento

- [ ] Identidade, fontes, universo e população qualificados.
- [ ] IVT existente validado ou cálculo necessário qualificado.
- [ ] Estado real da base cidadã e semântica cadastral explícitos.
- [ ] XLSX congelado e parser isolado aprovado; clusters e condições registrados.
- [ ] Quatro PNGs finais com hash, atribuição, Norte e legendas coerentes.
- [ ] Fonte efetiva, dados anteriores e limites da reversão conhecidos.
- [ ] Autorização de produção registrada; responsáveis definidos sem duplicidade.
- [ ] Release necessário publicado e confirmado, sem alterações alheias.
- [ ] Uma importação e quatro mapas reconciliados; fonte correta comprovada.
- [ ] Municípios anteriores preservados; evidência visual e seus limites registrados.
- [ ] Resultado final curto, sem melhorias adicionais para poder encerrar.
