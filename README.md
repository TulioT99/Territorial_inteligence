# Territorial_inteligence
# Expansão municipal dos dashboards ProDignus

Este diretório reúne o guia operacional para incluir novas cidades nos dashboards da ProDignus, incorporando os aprendizados de Goiânia, Porto Alegre, Curitiba e Florianópolis.

## Documento principal

[Guia Mestre de Expansão Municipal — v3](./ProDignus_Guia_Mestre_Expansao_Municipal_Dashboards_v3.md)

O guia orienta ChatGPT, Cursor e os responsáveis pela operação nas etapas de:

1. Diagnóstico de fontes IBGE, universo setorial, IVT e base cidadã.
2. Preparação e validação do XLSX, dos quatro mapas e das legendas.
3. Publicação e cargas autorizadas.
4. Reconciliação dos resultados e preservação dos municípios existentes.

Inclui um prompt reutilizável para iniciar uma nova cidade.

## Como utilizar

Antes de executar qualquer etapa, forneça o guia completo ao ChatGPT e ao Cursor, informando o município, a UF e o código IBGE.

Reutilize os scripts e artefatos qualificados. Agrupe preparação e validação em uma única entrega para autorização, evitando repetir verificações já concluídas sem evidência nova.

## Regras essenciais

- Preservar a metodologia IVT_V2 e distingui-la de IVT_C4O_V1.
- Não interpretar ausência de informação ou base cidadã vazia como cobertura populacional zero.
- Tratar XLSX e mapas como cargas independentes.
- Conferir município, arquivo e hash antes de cada envio.
- Não importar, excluir dados ou publicar código sem autorização para essas ações.
- Não repetir envios quando a resposta for ambígua.
- Confirmar a fonte efetivamente consumida pelo dashboard.
- Registrar limitações de acesso e evidência sem declarar verificações não realizadas.

## Documentos anteriores

O Guia Mestre v3 consolida e atualiza o Roteiro Mestre v2 e a Decisão de Escopo e Governança v2 para novas expansões municipais.

Documentos anteriores devem ser mantidos como histórico. Instruções temporárias de projetos antigos não devem ser reexecutadas automaticamente.

## Manutenção

Atualize o guia quando uma execução revelar uma correção ou aprendizado reutilizável. Registre a versão, a data e o motivo da alteração.

IDs de importações, versões publicadas e contagens históricas são referências de uma execução específica; não substituem a verificação do estado atual.

Não incluir credenciais, tokens, chaves de API ou dados pessoais neste diretório.
