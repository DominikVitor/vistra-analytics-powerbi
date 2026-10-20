# Checklist de publicação e privacidade

> Este pacote apresenta **capturas e documentação**. Ele não inclui o PBIX, PDFs ou bases de origem.

## Antes de publicar o repositório de imagens/documentação

- [x] As três capturas exibem rótulos fictícios de vistoriador (A e B).
- [x] Não aparecem endereços, telefones ou nomes de clientes nas capturas fornecidas.
- [ ] Revisar o avatar/ilustração e demais elementos de identidade no cabeçalho, se necessário.
- [ ] Revisar os textos e metadados dos arquivos antes de tornar o repositório público.
- [ ] Certificar-se de que há autorização para publicar o material e indicadores agregados de uma operação real.

## Se decidir publicar o arquivo `.pbix` posteriormente

- [ ] Criar uma **base de origem substituta**, sem informações pessoais, ao invés de depender apenas de etapas de "Substituir Valores".
- [ ] Revisar consultas M no Editor Avançado e passos aplicados (nomes originais podem estar em literais de substituição).
- [ ] Revisar caminhos locais, nomes de pastas, URLs, credenciais e parâmetros das conexões.
- [ ] Verificar tabelas, colunas, páginas e visuais ocultos, tooltips, medidas e descrições.
- [ ] Revisar registros brutos importados e eventuais chaves/IDs que possam possibilitar reidentificação por combinação com fontes externas.
- [ ] Rever dados financeiros e datas granularizadas que possam identificar clientes ou atividades.
- [ ] Verificar relações, atualização e resultados após trocar a fonte, incluindo o cálculo de comissão.
- [ ] Validar com os responsáveis pelos dados as permissões de compartilhamento.
- [ ] Reabrir e testar o arquivo `.pbix` final antes de disponibilizá-lo.

## Boas práticas

Alterar apenas os nomes visíveis constitui **pseudonimização**, não necessariamente anonimização. Uma captura pode não revelar informações pessoais enquanto o arquivo de modelo continua armazenando dados internos. A publicação deste pacote de capturas e README é uma alternativa mais segura até a revisão das origens e do PBIX.
