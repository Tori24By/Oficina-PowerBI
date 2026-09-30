# Oficina-PowerBI
# Primeira Tabela:

Dim_Calendario = 
VAR DataMinima = MIN(Fato_Vendas[Data_Pedido])
VAR DataMaxima = MAX(Fato_Vendas[Data_Pedido])
RETURN
ADDCOLUMNS (
    CALENDAR(DataMinima, DataMaxima),
    "Ano", YEAR([Date]),
    "Mês Num", MONTH([Date]),
    "Nome Mês", FORMAT([Date], "mmmm"),
    "Mês/Ano", FORMAT([Date], "mmm/yyyy"),
    "AnoMesNum", YEAR([Date]) * 100 + MONTH([Date]),
    "Trimestre", "T" & FORMAT([Date], "q"),
    "Dia da Semana", FORMAT([Date], "dddd")
)


# Tabela _Medidas:
Total Unidades = SUM(Fato_Vendas[Quantidade])
Total Pedidos = DISTINCTCOUNT(Fato_Vendas[ID_Pedido])
Total Custo = SUM(Fato_Vendas[Custo_Total])
Total Lucro = [Total Faturamento] - [Total Custo]
Total Faturamento = SUM(Fato_Vendas[Valor_Total])
Ticket Medio = DIVIDE([Total Faturamento], [Total Pedidos], 0)
Margem Lucro % = DIVIDE([Total Lucro], [Total Faturamento], 0)

Faturamento Ano Anterior = 
CALCULATE(
    [Total Faturamento], 
    SAMEPERIODLASTYEAR(Dim_Calendario[Date])
)

Crescimento Ano à Ano % = 
VAR FaturamentoAtual = [Total Faturamento]
VAR FaturamentoPassado = [Faturamento Ano Anterior]
RETURN
DIVIDE(FaturamentoAtual - FaturamentoPassado, FaturamentoPassado, 0)
