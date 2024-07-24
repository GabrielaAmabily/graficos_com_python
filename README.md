# Primeiro código do mini curso
# Revisão funções básicas

import pandas as pd
import matplotlib.pyplot as plt
import plotly.graph_objects as go
import geopandas as gpd

# Carregar a planilha que está no Google Drive
caminho_arquivo_excel = 'C:/caminho/para/seu/arquivo/tabela_MG_dem.xlsx'  # Copiar caminho
dados_excel = pd.read_excel(caminho_arquivo_excel)

print(dados_excel.head())

# Calcular estatística descritiva
est_desc = dados_excel.describe()
print(est_desc)

# Acessar dado específico na tabela
bh = dados_excel[dados_excel['Cidades'] == 'Belo Horizonte']
print(bh)

pcs = dados_excel[dados_excel['Cidades'] == 'Poços de Caldas']
print(pcs)

# Criar ranking
# Primeiro ordenar
dados_dem_ranking = dados_excel.sort_values(by='Pop', ascending=False)
print(dados_dem_ranking[['Cidades', 'Pop']].head(10))

# Plotar figura
top_10_populacao = dados_dem_ranking[['Cidades', 'Pop']].head(10)

plt.figure(figsize=(10, 6))
plt.bar(top_10_populacao['Cidades'], top_10_populacao['Pop'], color='skyblue')
plt.xlabel('Municípios')
plt.ylabel('População')
plt.title('Top 10 Municípios com maior população em MG')
plt.xticks(rotation=45, ha='right')
plt.tight_layout()
plt.show()

plt.figure(figsize=(8, 8))
plt.pie(top_10_populacao['Pop'], labels=top_10_populacao['Cidades'], autopct='%1.1f%%', colors=plt.cm.tab20.colors)
plt.title('Distribuição percentual das 10 maiores populações totais de MG')
plt.axis('equal')
plt.show()

fig = go.Figure(data=[go.Bar(x=top_10_populacao['Cidades'], y=top_10_populacao['Pop'], marker_color='skyblue')])
fig.update_layout(
    title='Top 10 Municípios mais populosos',
    xaxis_title='Municípios',
    yaxis_title='População',
    xaxis_tickangle=-45
)
fig.show()

# Trabalhando com arquivos shapefile
caminho_arquivo_shp = 'C:/caminho/para/seu/arquivo/MG_Malha_Preliminar_2022/MG_Malha_Preliminar_2022.shp'
dados_shp = gpd.read_file(caminho_arquivo_shp)
print(dados_shp.head())

# Combinando os dados
dados_combinados = pd.merge(dados_shp, dados_excel, left_on='NM_MUN', right_on='Cidades', how='inner')
print(dados_combinados.head())

# Plotando mapas
plt.figure(figsize=(10, 8))
dados_combinados.plot(column='Densidade demog', cmap='Reds', legend=True)
plt.xlabel('Longitude')
plt.ylabel('Latitude')
plt.title('Mapa densidade demográfica')
plt.show()

plt.figure(figsize=(10, 8))
dados_combinados.plot(column='Pop', cmap='Blues', legend=True)
plt.xlabel('Longitude')
plt.ylabel('Latitude')
plt.title('Mapa população dos municípios')
plt.show()

plt.figure(figsize=(10, 8))
dados_combinados.plot(column='Area da unidade territorial', cmap='Greens', legend=True)
plt.xlabel('Longitude')
plt.ylabel('Latitude')
plt.title('Maiores áreas')
plt.show()
