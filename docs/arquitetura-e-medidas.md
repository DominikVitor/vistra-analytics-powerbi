# Documentação técnica — VISTRA Analytics

## Fluxo dos dados

1. **Relatórios em PDF**: arquivos contendo registros de vistorias por período.
2. **Power Query**: extração e combinação de dados de relatórios, tratamento dos tipos das colunas e padronização dos nomes de vistoriadores.
3. **Consultas auxiliares**: `Descontos`, `Tabelas de descontos` e `Tipos de descontos` para registros e referências financeiras.
4. **Modelo do Power BI**: dimensão de vistoriadores (`Dim_Vistoriadores`), dimensão de datas e tabelas de fatos.
5. **DAX**: medidas de faturamento, desconto, comissão e produtividade.
6. **Relatório**: três páginas com interações e segmentações sincronizadas.

## Dimensão de vistoriadores

A dimensão é formada pelos nomes presentes nos relatórios e na tabela de ocorrências de descontos, eliminando repetições.

```dax
Dim_Vistoriadores =
DISTINCT(
    UNION(
        SELECTCOLUMNS(
            'Relatorios',
            "Vistoriador", 'Relatorios'[Vistoriador]
        ),
        SELECTCOLUMNS(
            'Descontos',
            "Vistoriador", 'Descontos'[Vistoriador]
        )
    )
)
```

- `SELECTCOLUMNS`: seleciona e nomeia a coluna desejada de cada origem.
- `UNION`: une as linhas das duas listas.
- `DISTINCT`: elimina valores repetidos.

## Comissão sobre valor líquido

```dax
Comissao Gerada =
SUMX(
    VALUES('Dim_Vistoriadores'[Vistoriador]),
    VAR Profissional = 'Dim_Vistoriadores'[Vistoriador]
    RETURN
        IF(
            Profissional = "Vistoriador B",
            [Valor Liquido] * 0.10,
            0
        )
)
```

- `VALUES`: retorna os vistoriadores distintos no contexto do visual.
- `SUMX`: avalia cada vistoriador e soma os resultados.
- `VAR` / `RETURN`: define e utiliza a variável `Profissional`.
- `IF`: calcula 10% do valor líquido apenas quando a produção é do Vistoriador B; nos demais casos, retorna zero.

**Conferência do recorte das capturas:** R$ 11.766,98 × 10% = R$ 1.176,698, exibidos como **R$ 1.176,70** por formatação.

## Cascata de faturamento

A visualização utiliza uma tabela de etapas com as categorias de receita bruta e descontos. A medida retorna a receita bruta como aumento e os descontos como redução. O gráfico de cascata exibe o saldo acumulado de **R$ 31.431,86**.

```dax
Etapas Financeiras =
DATATABLE(
    "Etapa", STRING,
    "Ordem", INTEGER,
    {
        {"Faturamento Bruto", 1},
        {"Descontos", 2}
    }
)
```

- `DATATABLE`: cria uma pequena tabela calculada com valores declarados no código.
- `STRING`: especifica o tipo texto para a coluna de categoria.
- `INTEGER`: especifica o tipo inteiro para a coluna de ordenação.

## Regras e cuidados

- Descontos efetivamente aplicados vêm da tabela de ocorrências `Descontos`; a tabela `Tipos de descontos` armazena os valores de referência.
- Os R$ 34,00 apresentados correspondem a uma ocorrência de **No Show** no recorte exibido.
- Uma comissão **gerada** pela produção de determinado vistoriador não deve ser confundida com pagamento recebido por esse mesmo profissional.
- Números e resultados apresentados nas capturas correspondem ao estado de filtros mostrado, não ao comportamento de um feed em tempo real.
- Os nomes originais foram substituídos por **Vistoriador A** e **Vistoriador B**, mas etapas do Power Query podem preservar literais originais; revisar antes de compartilhar o PBIX.

## Validações funcionais realizadas

- Navegação entre as três páginas.
- Sincronização de segmentações de período entre páginas.
- Interações de visuais e matrizes ao selecionar vistoriadores.
- Botões de reset para restaurar filtros.
- Conferência do faturamento: **R$ 31.465,86 − R$ 34,00 = R$ 31.431,86**.
- Conferência da comissão produzida por **Vistoriador B**: **R$ 1.176,70**.
