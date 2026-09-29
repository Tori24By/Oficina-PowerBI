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
